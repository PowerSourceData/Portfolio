# Systemic Moat Test Framework (Prompt-Template)

## 1. Zielsetzung & Qualitätskriterien (Definition of Done)

Erstelle eine harte, logische, zukunftsorientierte und von Marketing-Narrativen befreite qualitative Unternehmensforschung und ökonomische Systemanalyse. Das Ziel ist die Bestimmung der strukturellen Unersetzbarkeit eines Unternehmens sowie die kinetische Veränderung (Evolution/Erosion) seines Burggrabens über den Zeitverlauf.

### 🔴 Qualitäts-Leitlinien (Strict Compliance)
* **The Honesty Protocol:** Wenn Daten fehlen oder spekulativ sind, setze den entsprechenden Wert zwingend auf `N/A`. Überoptimismus wird dreimal schwerer bestraft als ein ehrliches `N/A` (im Zweifel für die Vorsicht entscheiden). Füge bei Datenknappheit eine explizite Sektion **"Daten-Lücken & Unsicherheiten"** ein.
* **Source & Logic Labeling:** Deklariere bei jedem einzelnen Scoring-Punkt zwingend: 
  * `[Basis: Fakt]` (empirisch belegt) 
  * `[Basis: Ableitung]` (logische Schlussfolgerung)
* **Sprache & Format:** Deutsch (Standard). Nutze rein maskuline oder neutrale Formen (keine Gendersprache). Finale Scores im deutschen Zahlenformat mit Komma (z. B. `8,50`).

---

## 2. Analyse-Schritte

### 🟩 MANDATORY STEP 0 — DIE 6 MOAT TESTS & BURGGRABEN-KINETIK

#### A. Statisches Scoring
Gib für jeden Wert die Begründung inklusive Datenbasis `[Basis: Fakt/Ableitung]` an.

| Kennzahl | Test-Name | Skalen-Definition |
| :--- | :--- | :--- |
| **S1** | **Substitution Test** | 5 = Unmöglich, 1 = Sofort |
| **S2** | **Source of Enforcement** | Level 1: Gesetz/Infrastruktur bis Level 6: Preis/Exzellenz |
| **S3** | **Replacement Time Test** | 5 = >10 Jahre, 1 = <1 Jahr |
| **S4** | **Capital & Know-How Test** | 5 = Jahrzehnte + >10 Mrd. $, 1 = Minimal |
| **S5** | **Demand Enforcement Test** | 5 = Systemkritisch, 1 = Rein optional |
| **S6** | **Moat Decay Test** | 0 = Kein Risiko, 4 = Strukturell unvermeidbar |

#### B. Moat Momentum (Vektor-Analyse)
Bestimme die kinetische Stoßrichtung des Burggrabens im Vergleich zum Vorjahr oder der jüngsten strategischen Entwicklung und ordne exakt einen dieser Vektoren zu:
* **`[Expansion]`**: Der Burggraben mutiert aktiv von einem austauschbaren Punkt-Produkt/Feature zu einer zentralen Plattform, einem Standard oder einer kritischen Infrastruktur.
* **`[Stagnation]`**: Der Burggraben ist tief, aber statisch. Keine nennenswerte Expansion in neue Wertschöpfungsstufen.
* **`[Erosion]`**: Technologischer Wandel, Open-Source-Druck oder Commodity-Gefahr weichen den Graben auf.

---

### 🟩 MANDATORY STEP 1 — SYSTEM-ROLLE & HIERARCHIE
* **Rollen-Zuordnung:** Ordne das Unternehmen einer klaren Rolle zu (z. B. *"SR-1 Global Choke Point"*).
* **Core-Eligible Check:** Validiere mathematisch: Ist `S1 ≥ 4` UND `S2 ≤ Level 3`?

---

### 🟩 MANDATORY STEP 2 — STRATEGISCHE ANALYSE & EVOLUTION
* **Business Model & Value Chain (Invasions-Check):** Kolonisiert das Unternehmen neue Stufen der Wertschöpfungskette des Kunden oder stagniert das Ökosystem?
* **Problem Solved & Customer Dependence (Lock-in-Drift):** Verschiebt sich die Abhängigkeit des Kunden von "technisch mühsam" hin zu "betriebswirtschaftlich unersetzbar"?
* **The "What If" Scenario:** Welche Systemfolgen entstehen beim hypothetischen Verschwinden des Unternehmens zum aktuellen Zeitpunkt?
* **Portfolio Redundancy Check:** Systematische Prüfung auf ähnliche Firmen oder aufstrebende disruptive Angreifer.

---

### 🟩 MANDATORY STEP 3 — SCORING & KLASSIFIZIERUNG
Berechne die folgenden Kennzahlen (Format: `0,00` mit Komma):
* **Aggregated Moat Score:** `(S1 × 0,6) + (S_Avg × 0,4)` (wobei `S_Avg` der Durchschnitt von S2 bis S5 ist)
* **Decay-Adjusted Score:** `Aggregated - (S6 × 0,25)`
* **Klassifizierung:** Ordne exakt eine Kategorie zu: `Core-eligible`, `Pre-Core`, `Satellite-only` oder `Not suitable`.

---

## 3. Daten-Export & Audit-Protokoll

### 📑 MANDATORY AUDIT STEP (Strikte Ausgabekontrolle)
Bevor der finale Code-Block ausgegeben wird, muss intern folgendes Protokoll abgearbeitet und das Ergebnis textuell ausgegeben werden:
1. **Spalten-Zählung:** Zähle die Anzahl der generierten Tabulatoren in der Export-Zeile. Es müssen exakt 13 Tabstopps (für 14 Spalten) vorhanden sein. Melde: *"Audit: [Anzahl] von 14 Spalten validiert."*
2. **Formel-Check:** Bestätige, dass die mathematische Berechnung für den *Aggregated Score* und den *Decay-Adjusted Score* exakt nach den Formeln aus Schritt 3 durchgeführt wurde.
3. **Zeichenbegrenzung:** Bestätige, dass Spalte 14 (Takeaway) die Grenze von 120 Zeichen nicht überschreitet.

### 💾 OUTPUT & EXPORT-Schnittstelle
Anweisungstext direkt vor dem Code-Block: 
> "Kopiere die folgende Zeile und füge sie in Zelle A[n] deines Sheets 'Moat Scoring' ein:"

Beende die Analyse mit einem Code-Block, der **EXAKT eine einzige Zeile** enthält. Diese Zeile muss tabulatorgetrennt (TSV) sein.

**Spalten-Reihenfolge (Tab-getrennt):**
`Datum (TT-MM-JJJJ)` | `2. Unternehmen` | `3. System-Rolle (Volltext)` | `4. S1 Score` | `5. S2 Level` | `6. S3 Score` | `7. S4 Score` | `8. S5 Score` | `9. S6 Score` | `10. Vektor-Trend` | `11. Aggregated Score` | `12. Decay-Adjusted Score` | `13. Klassifizierung` | `14. Strategisches Takeaway (Max. 120 Zeichen)`

---
⚠️ *Mandatory Disclaimer: Dies ist keine Anlageberatung. Die Inhalte dienen ausschließlich Informations- und Bildungszwecken.*
