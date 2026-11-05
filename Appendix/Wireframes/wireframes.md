# Wireframes (ASCII — replace with Figma/screenshots later)

## Screen 1 — Upload / capture
```
+----------------------------------------------------------+
| CardamomLens          Site: [Puttady ▼]   Grader: [____] |
| ⚠ Screening category — not an official AGMARK grade      |
+----------------------------------------------------------+
|  Sample ID: [ S-0231 ]   Lot ID: [ L-77 ]                |
|  +----------------------+   Tips:                        |
|  |  Drop photo here     |   • plain white background     |
|  |  or [Browse]         |   • even light, no flash glare |
|  +----------------------+   • single layer of pods       |
|                         [ Analyse sample ]               |
+----------------------------------------------------------+
```
## Screen 2 — Result (high confidence)
```
+----------------------------------------------------------+
| Photo          | Heatmap (Grad-CAM)                      |
| [image]        | [overlay]                               |
+----------------------------------------------------------+
| Category: STANDARD      Confidence: HIGH (0.91)          |
| Top-2: Standard 0.91 | Premium 0.06                      |
| Recommendation:  ✅ ACCEPT FOR SCREENING                  |
| Why: confidence ≥ 0.85, margin ok, no defect signal      |
| [ Confirm ]   [ Override… ]                              |
+----------------------------------------------------------+
```
## Screen 3 — Result (low confidence / defect)
```
+----------------------------------------------------------+
| Category: DEFECTIVE?    Confidence: LOW (0.52)           |
| Top-2: Defective 0.52 | Low_Grade 0.38                   |
| Recommendation:  🔍 MANUAL REVIEW REQUIRED               |
| Why: defect class / low confidence                       |
| Your inspection result: ( ) Premium ( ) Standard         |
|                         ( ) Low_Grade ( ) Defective      |
| Reason (required): [____________________________]        |
| [ Save decision ]       (Accept disabled)                |
+----------------------------------------------------------+
```
## Screen 4 — Supervisor queue
```
Overrides this week: 14   Disagreement rate: 9%   Seeded-error catch: 80%
| Time | Sample | AI | Human | Reason | Grader |
```
(Numbers above are placeholder mock data.)

## Key journey
Upload → Analyse → (Accept path: confirm) | (Review path: inspect → record → save) → logged.
