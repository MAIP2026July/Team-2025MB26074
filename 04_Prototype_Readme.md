## Prototype / MVP — CardamomLens

### Prototype Type
Streamlit web app (Python) + training script. Local run: see `README.md`.

### What It Demonstrates
The core decision loop: **image in → category + confidence + (Grad-CAM) → routed recommendation → human confirm/override with mandatory reason → append-only audit log.**
Also: configurable thresholds (`prototype/config.yaml`), image-quality gate, defect-class forced review, optional blind mode, manual-only fallback on model error.

### AI Role
Image classifier (MobileNetV2 transfer learning, temperature-scaled confidence) produces probabilities; **routing is deterministic** (`routing.py`) so it is testable and auditable. Grad-CAM explains the prediction.

### Two run modes (be clear about which you demo)
| Mode | When | What it is |
|---|---|---|
| **DEMO** | no `models/cardamomlens.pt` | Colour/darkness heuristic — *not AI*; shows the workflow and acts as the **rules baseline** |
| **MODEL** | after `python train.py --data data` | Trained classifier + Grad-CAM |

### What It Does Not Yet Prove
- Real-world accuracy (no representative labelled data yet) or cross-site/lighting generalisation
- Equivalence to AGMARK grades (AGEB/AGB/AGS)
- Economic value at scale; ERP/auction-system integration
- That graders resist automation bias (needs pilot with blind mode)

### Tests
`pytest prototype/tests` — covers routing: confident accept, defect-forced review, low confidence, poor quality, model missing.

### Links
- Repository: https://github.com/MAIP2026July/Team_26074
- Live demo: *(optional — deploy on Streamlit Community Cloud and paste link)*
- Screenshots / Video: `Appendix/Screenshots_or_Video/` *(add after running)*
