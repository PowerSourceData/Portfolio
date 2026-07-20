# 🔍 Performance Optimization Playbook: Microsoft Fabric (T-SQL)

Dieses Playbook dient als methodischer Leitfaden zur Diagnose und Behebung von Performance-Bottlenecks in Microsoft Fabric (Synapse Data Warehouse & Fabric SQL Database) mittels T-SQL-Ausführungsplänen und Dynamic Management Views (DMVs).

---

## 🛠️ 1. Werkzeuge & Telemetrie-Abruf

### Visueller Ausführungsplan
1. Öffne den **Fabric SQL-Editor** oder das SQL Server Management Studio (SSMS) / Azure Data Studio.
2. Aktiviere die Option **"Inkludiere Live-Ausführungsplan"** (Include Actual Execution Plan) vor der Ausführung.
3. Fokus auf die **Pfeildicke** (repräsentiert Datenvolumen) und die **Operator-Kosten in %** (Hotspots).

### DMV-basierte Analyse (Synapse Engine)
Für im Hintergrund laufende oder bereits abgeschlossene Abfragen im Data Warehouse nutzen wir die verteilten DMVs:

```sql
-- 1. Identifikation der aktivsten/langlaufenden Abfragen
SELECT 
    request_id,
    status,
    submit_time,
    start_time,
    total_elapsed_time AS duration_ms,
    data_scanned_remote_storage_mb, -- Wichtig für OneLake Cold-Start Analyse
    command
FROM sys.dm_pdw_exec_requests
WHERE status IN ('Running', 'Completed')
ORDER BY total_elapsed_time DESC;

-- 2. Analyse von Datenbewegungen (DMS) für eine spezifische request_id
SELECT 
    request_id,
    step_index,
    operation_type,
    distribution_type,
    location_type,
    status,
    total_elapsed_time AS duration_ms
FROM sys.dm_pdw_request_steps
WHERE request_id = 'QIDXXXXXX' -- Hier Request-ID einsetzen
ORDER BY step_index;
```

---

## 📉 2. Diagnose-Matrix (Top Bottlenecks)

### A. Der "Cold Start" Effekt (Erster Abfrage-Aufruf)
* **Symptom:** Der erste Lauf einer Abfrage dauert 10x länger als der zweite. In `sys.dm_pdw_exec_requests` zeigt `data_scanned_remote_storage_mb` beim ersten Lauf hohe Werte, beim zweiten Lauf nahezu Null.
* **Ursache:** Die Daten wurden beim ersten Aufruf aus dem OneLake (Remote Storage) in den lokalen Cache der Compute-Knoten geladen.
* **Behebung:** 
  * Für Benchmark- und Optimierungszwecke **immer den 2. oder 3. Lauf analysieren**, um den reinen Abfrageplan ohne Cache-Initialisierung zu bewerten.
  * In Produktion: Regelmäßige Abfragen (Dashboards) wärmen den Cache automatisch.

### B. Data Movement Service (DMS) Overheads
Da Fabric ein verteiltes Datenbanksystem ist, müssen Daten für Joins oft zwischen Knoten verschoben werden.
* **Symptom im Plan:** Hohe Kosten bei Operatoren wie `ShuffleMove`, `BroadcastMove` oder `PartitionMove`.
* **Ursache:** Zwei große Tabellen werden über Spalten gejoint, die nicht identisch verteilt sind oder keine passenden Statistiken aufweisen.
* **Behebung:**
  * Reduziere die Zeilenanzahl **vor** dem Join durch frühzeitige Filter (`WHERE`-Clause).
  * Verwende Aggregate (`GROUP BY`) vor dem Join, falls möglich, um die Datenmenge für den DMS zu minimieren.

### C. Cardinality Mismatch (Falsche Zeilenschätzung)
* **Symptom:** Die *Estimated Number of Rows* (Erwartete Zeilen) weicht massiv (Faktor 100+) von den *Actual Number of Rows* (Tatsächliche Zeilen) ab. Der Optimizer wählt dadurch suboptimale physische Operatoren (z.B. Nested Loops statt Hash Joins).
* **Ursache:** Veraltete oder fehlende Spaltenstatistiken.
* **Behebung:**
  * Manuelles Update der Statistiken erzwingen:
    ```sql
    UPDATE STATISTICS dbo.MeineTabelle WITH FULLSCAN;
    ```
  * *Hinweis zu Fabric:* Fabric erstellt Statistiken automatisch im Hintergrund (Auto-Stats). Bei massiven Datenänderungen innerhalb kurzer Zeit (ETL/ELT) kann es jedoch zu Verzögerungen kommen. Hier ist ein explizites `UPDATE STATISTICS` im ETL-Pipeline-Abschluss Best Practice.

### D. Non-Enforced Constraints Einschränkung
* **Symptom:** Der Optimizer führt redundante Joins aus, obwohl eine logische 1:n Beziehung vorliegt.
* **Ursache:** Primary Key und Unique Constraints werden in Fabric Data Warehouse deklariert, aber **nicht erzwungen (non-enforced)**. Der Engine verlässt sich beim Erstellen des Ausführungsplans nicht blind darauf.
* **Behebung:** Join-Prädikate im SQL-Code explizit und präzise definieren. Redundante Tabellen-Joins manuell eliminieren, da der Optimizer diese nicht automatisch wegoptimiert.

---

## 📋 3. Methodischer Ablaufplan (Checkliste)

```
[1. Identifikation]  --> Langläufer via sys.dm_pdw_exec_requests isolieren.
        |
[2. Cache-Check]     --> data_scanned_remote_storage_mb prüfen (Ausschluss Cold Start).
        |
[3. Plan-Analyse]    --> Live-Ausführungsplan öffnen -> Prozentuale Hotspots isolieren.
        |
[4. DMS & Stats]     --> Hohe DMS-Kosten? -> Filter vorziehen. Zeilen-Mismatch? -> UPDATE STATISTICS.
        |
[5. Validierung]     --> Abfrage erneut ausführen (Lauf 2/3) und Ressourcen-Delta messen.
```
