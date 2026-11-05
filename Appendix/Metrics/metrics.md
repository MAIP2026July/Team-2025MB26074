# Metrics & Go/No-Go (all targets are *assumptions* to be agreed with a domain expert)

## Metric tree
North star: **grader minutes saved per 100 samples without increasing false-accepts**
```
North star
├─ Product:  % routed to Accept | time/sample | % overrides
├─ Model:    macro-F1 | Defective recall | ECE | gain over rules baseline
├─ Business: pilot→paid conversion | revenue/site | rejections avoided (pilot est.)
└─ Guardrail: false-accept rate (Defective) | seeded-error catch rate | Grad-CAM credibility | per-site performance gap
```
## Gates
| Gate | Pass condition |
|---|---|
| G1 Prototype→Pilot | Macro-F1 ≥ rules baseline +10 pts; Defective recall ≥ 0.90; ECE ≤ 0.10 |
| G2 Pilot→Paid roll-out | FA(Defective) ≤ 2%; ≥ 30% auto-routed Accept; trust ≥ baseline; ≥ 70% Grad-CAMs "credible" |
| G3 Scale | AGMARK validity study done; cross-site gap ≤ 5 pts; drift monitor live |
| STOP | Model ≈ baseline; or FA ceiling unattainable; or categories rejected by experts |

## Pilot instrumentation
Log per sample: AI class/prob, route, human final, override reason, time-to-decision, image hash, model/threshold version. Weekly review of confidence histogram & override rate by site/lighting.
