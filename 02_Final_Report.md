# CardamomLens — Final Report
*MBA ZG583 Capstone · Avinash Kumar Jha (individual)*

> Legend: **(A)** = planning assumption to validate. No measured results are claimed.

---
## 1. Product Strategy and Opportunity Framing

### Product Vision
Make cardamom grading **consistent, explainable and auditable** by giving every grader an AI second opinion — while keeping the human as the final authority.

### JTBD
When I am grading a high volume of cardamom samples under time pressure with variable lighting and fatigue,
I want to quickly know which samples are clearly fine and which need careful inspection,
so that I can grade faster, stay consistent, and defend my decision if it is contested.

*Secondary JTBD (export QA):* When I am clearing a lot for shipment to a strict market, I want a documented, repeatable pre-check that flags defect risk, so that I avoid rejection and can show due diligence.

### Opportunity Framing
- **Why it matters:** grades set farmer payment and export eligibility; disputes and rejections are expensive.
- **Why current alternatives are weak:** manual grading = slow, variable, no record. Rule thresholds = brittle across conditions. Generic vision APIs = no cardamom-specific categories.
- **Why AI may help:** learns combined visual cues; produces calibrated confidence enabling *triage* (the real value is reducing full-inspection load, not replacing the grader).

### Opportunity Matrix
| Idea | Business Value | Feasibility | Priority |
|---|---|---|---|
| Confidence-routed quality screening (image → category + action) | High | Medium (data-limited) | **MVP** |
| Grad-CAM explanations for trust | Medium | High | **MVP** |
| Grader override + reason audit trail | High | High | **MVP** |
| Defect flagging (mould/thrips) as hard-review trigger | High | Medium | **MVP (as flag)** |
| Pre-shipment QA report (PDF, per lot) | High (revenue) | High | Phase 2 |
| Lot-level sampling statistics | Medium | Medium | Phase 3 |
| Official AGMARK grade prediction | Very high | Low (label validity unproven) | **Not now** |
| Farmer-facing grade app | Medium | Low (no control of grading step) | Rejected |
| ERP / auction-system integration | High | Low (access, time) | Phase 3 |

### RICE (top candidates) — *(A) scores*
| Feature | Reach | Impact | Confidence | Effort (person-wks) | RICE |
|---|---|---|---|---|---|
| Confidence routing | 8 | 3 | 70% | 2 | 8.4 |
| Override + audit log | 8 | 2 | 90% | 1 | 14.4 |
| Grad-CAM | 8 | 1 | 80% | 1 | 6.4 |
| Lot PDF report | 5 | 2 | 80% | 1.5 | 5.3 |
| ERP integration | 9 | 3 | 30% | 8 | 1.0 |

### Build vs Buy vs Partner/API
- **Recommended path:** **Build** a lightweight custom model (MobileNetV2/EfficientNet-B0 transfer learning) + **build** the confidence/routing workflow. Use open-source frameworks; no paid vision API for the core classification.
- **Why:** generic vision APIs don't know cardamom quality classes; differentiation = domain classification + routing logic + audit trail. Transfer learning needs few labelled images and cheap CPU/GPU.
- **Trade-offs:** (+) domain fit, low per-inference cost, data moat over time; (−) we own data collection, label quality, MLOps. **Fallback:** if data is too thin, run a *Wizard-of-Oz* pilot (expert labels + dashboard) to earn data and revenue first.
- **Platform vs Feature:** start as a *feature/tool* for graders; platform ambitions (multi-spice, multi-site) deferred.

### Course case lens — Instacart/Tesla (lifecycle & drift) and iMerit (labelling quality)
| Question | Answer |
|---|---|
| Similar? | Both depend on label quality and degrade when real-world conditions drift (new lighting, season, variety). |
| Different? | Our unit of value is *triage*, not full automation; stakes are per-lot financial rather than safety. |
| Lesson that transfers | Treat labels as a product: inter-grader agreement and re-labelling loops matter more than model architecture. Monitor drift per site/season. |
| What we'd do differently | Start with an expert-adjudicated gold set and measure grader disagreement *before* training. |

### First Wedge
**One site, one use case:** pre-shipment screening of small cardamom lots at **one export-QA team (or one aggregator)** — photo of a sample → screening category + confidence + action. Auction-centre rollout follows after label-validity work.

---
## 2. PM Artifact Pack
(Full docs: `Appendix/PRD/PRD.md`, `Appendix/Metrics/metrics.md`, `Appendix/Roadmap/roadmap.md`.)

### Problem Statement
Graders at auction centres and export QA teams classify cardamom samples by eye. The decision (which grade-band / whether a sample is defect-free) is repeated thousands of times, varies by grader, and is unrecorded. We need to **speed up and standardise the first-pass screening decision** and **create an audit trail**, without removing human authority.

### Product Vision
See §1.

### PRD Summary
- **User problem:** slow, inconsistent, non-auditable grading.
- **Core workflow:** upload → predict → confidence routing → human decision (+reason) → log.
- **AI role:** 4-class image classifier + confidence + Grad-CAM; deterministic routing rules on top.
- **Risks:** label mismatch vs AGMARK, non-representative data, automation bias, biased labels.
- **Fallback logic:** model unavailable / image quality poor / confidence low → *Manual review*; nothing ever auto-certifies.

### Feature Prioritization
| Feature | User Value | Effort | Confidence | Priority |
|---|---|---|---|---|
| Image upload + prediction + confidence | High | Medium | Medium | P0 |
| Confidence-band routing (configurable thresholds) | High | Low | High | P0 |
| Override with mandatory reason | High | Low | High | P0 |
| Audit log (CSV/SQLite) | High | Low | High | P0 |
| Grad-CAM heatmap | Medium | Medium | Medium | P1 |
| Image-quality gate (blur/low light) | Medium | Low | Medium | P1 |
| Lot-level PDF report | High ($) | Medium | High | P1 (revenue) |
| Multi-site dashboard | Medium | High | Low | P2 |
| ERP integration | High | High | Low | P3 |

### MVP Definition
Image in → category, confidence, heatmap, recommended action (Accept / Manual review) out; grader can override with reason; all events logged. **It proves the trust/workflow model, not production accuracy.**

### Roadmap (detail in `Appendix/Roadmap/roadmap.md`)
- **Phase 0 (wk 1–2):** 5–8 grader/QA interviews; collect/curate data; expert gold labels.
- **Phase 1 (wk 3–6):** model + prototype; baseline comparison; internal demo.
- **Phase 2 (wk 7–12):** **paid pilot** (1 site) + lot PDF report → first revenue.
- **Phase 3 (wk 13+):** subscription for 2–3 sites; label-validity study vs AGMARK; integrations.

### Success Metrics
- **Product:** % samples auto-routed to *Accept-for-screening* without later human reversal; time per sample.
- **Business:** grader minutes saved per 100 samples; pilot conversion to paid.
- **Trust/Guardrail:** false-accept rate on defective samples; override rate; automation-bias check (agreement on seeded wrong predictions).

### How PM artifacts changed because this is an AI product
| Standard artifact | AI-specific change here |
|---|---|
| Problem statement | Defines the *decision* (screening category + route), not a feature. |
| User story | Includes error cost: a false accept of a mouldy lot ≫ a false review. |
| PRD | Adds data assumptions, confidence thresholds, fallback, label-validity caveat. |
| Roadmap | Phases gated by *proof gates* (beats baseline; FA-rate ceiling), not dates only. |
| Metrics | Links model metrics (recall on defects) → business (minutes saved, rejections avoided). |
| Launch checklist | Review, rollback to manual-only, escalation, drift monitoring. |
| Pricing | Accounts for inference + review + labelling cost. |

---
## 3. UX, Workflow and Trust Design
(Screens: `Appendix/Wireframes/wireframes.md`; swimlane: `Appendix/Workflow/swimlane.puml`.)

### Main Workflow
1. Grader captures photo of sample on plain background (guidance overlay).
2. System runs image-quality gate → classifier → Grad-CAM.
3. Router applies thresholds → **Accept-for-screening** or **Manual review**.
4. Grader sees result with confidence + heatmap, confirms or overrides (reason required).
5. Decision and metadata written to audit log. Official grade is assigned by the human outside the model's category set.

### Trust and Explainability
- **Visible:** category, confidence band (not raw % alone), heatmap, reason for routing ("low confidence", "defect class", "poor image"), "screening only — not an AGMARK grade" banner.
- **Hidden by default:** raw logits, model version internals (available in "details").
- **Anti-automation-bias:** randomly seeded *challenge samples* in pilot; reviewer must record a reason *before* seeing AI suggestion on a sampled subset (blind mode).

### Uncertainty Handling
- **High confidence (≥ accept threshold, non-defect class):** "Accept-for-screening" — one-tap confirm; still logged.
- **Medium / boundary:** shows top-2 classes, recommends manual review.
- **Low confidence, defect class, or poor image:** forced *Manual review*; accept button disabled until grader records inspection result.
- **Failure/fallback:** model error → manual-only mode banner; log reason.

### Human-in-the-Loop
Human decides final grade; can override any recommendation (reason mandatory); senior grader sees queue of overrides and disagreements for calibration.

---
## 4. Data, Model, and Evaluation Strategy

### Problem as AI Task
Multi-class **image classification** (4 screening categories: `Premium`, `Standard`, `Low_Grade`, `Defective`) + **confidence-based triage**. *(A: class set to be finalised with an expert.)*

### Data Sources
- **External:** public cardamom image datasets (e.g., on Kaggle/Mendeley — *verify licence and label meaning before use*).
- **Faculty/partner:** small labelled set if provided.
- **User-generated (pilot):** photos + grader decisions + overrides (consent required).
- **Internal:** none today.

### Data Assumptions *(A)*
≥ 300–500 images per class for transfer learning to be meaningful; images on plain background; multiple lighting conditions. Public sets are likely lab-style and **not representative** of auction-floor conditions → hence a pilot-collected validation set is mandatory before any claim.

### Labelling / Ground Truth
Expert-adjudicated gold subset (≥ 2 graders; measure Cohen's κ). Disagreements go to senior grader. Labels are *screening categories*; mapping to AGMARK (AGEB/AGB/AGS) is a **separate validation study**, not assumed.

### Baseline
(1) **Rules baseline:** colour/darkness/pale-pixel thresholds (implemented in `prototype/classifier.py` demo mode). (2) **Majority-class** baseline. (3) Inter-grader agreement as human ceiling.

### Model / API Choice
Transfer-learned MobileNetV2 (fast on CPU, small). Temperature scaling for calibration. Grad-CAM for explanation. Rejected: paid vision API (no domain classes, recurring cost, data leaves our control); training from scratch (data-hungry).

### Evaluation Framework
| Metric Type | Metric | Why It Matters |
|---|---|---|
| Technical | Macro-F1, per-class recall (esp. `Defective`), confusion matrix | Detect where grades get confused |
| Technical | Calibration (ECE), reliability curve | Confidence routing only works if confidence is meaningful |
| Technical | Gain over rules baseline | Justifies "why AI" |
| Technical | Per-condition slices (light, camera, variety) | Generalisation across sites |
| Business | % samples routed to Accept; grader minutes saved/100 samples | Value of triage |
| Business | Rejection-risk catches per 100 lots (pilot) | Export value |
| Trust/Guardrail | **False-accept rate** on `Defective` | Highest-cost error |
| Trust/Guardrail | Override rate & reasons; seeded-error detection rate | Automation bias / drift |
| Trust/Guardrail | Grad-CAM expert credibility rating (1–5) | Explanation sanity |

### Launch Thresholds (go / no-go) *(A — to be agreed with domain expert)*
| Gate | Criterion |
|---|---|
| G1 (prototype → pilot) | Macro-F1 materially above rules baseline on held-out set (target: ≥ +10 pts); `Defective` recall ≥ 0.90 at the chosen accept threshold; ECE ≤ 0.10 |
| G2 (pilot → paid roll-out) | In pilot: false-accept on `Defective` ≤ 2%; ≥ 30% of samples auto-routed to Accept; grader-reported trust not lower than baseline; expert rates ≥ 70% of Grad-CAMs "credible" |
| G3 (scale) | Label-validity study vs AGMARK completed; cross-site performance within 5 pts; drift monitoring live |
| **Stop** | Model ≈ rules baseline, or FA-rate can't be held under ceiling |

---
## 5. Business, Economics, and Scaling

### Monetization Model
Hybrid: (a) **paid pilot fee** (fixed, per site, 6–8 weeks); (b) **per-site monthly subscription** (includes N samples) + overage; (c) **per-lot certified-style QA report** (non-official "screening report") for exporters. Government/auction bodies later via enterprise/tender.

### Value Logic
Grader time saved on clear cases → throughput; fewer disputes via audit trail; avoided rejections for exporters; consistent screening across sites.

### Cost Drivers
- **Model/inference:** minimal (CPU, on-prem or small cloud VM).
- **Data/labelling:** largest early cost (expert time).
- **Review cost:** humans still review low-confidence share.
- **Support/onboarding:** training graders, device/lighting setup.
- **Compliance/hosting:** data retention, audit storage.

### Unit Economics View *(illustrative assumptions — to be validated)*
| Item | Assumption |
|---|---|
| Samples screened / site / month | 3,000 |
| Share auto-routed to Accept | 40% |
| Grader time saved per auto-routed sample | 1.5 min |
| Minutes saved / month | 1,800 min = 30 hrs |
| Value at ₹300/hr loaded cost | ₹9,000/month (grader time only) |
| Inference + hosting cost | ≈ ₹1,000–2,000/month |
| **Implication** | Time-saving alone is a *thin* value case → pricing must anchor on **rejection avoidance and dispute reduction for exporters**, not grader minutes. This is why the wedge is export-QA, not auction floor. |

### Scaling Risks
Latency (low; on-device/CPU OK); cost growth (labelling and support, not inference); workflow complexity (per-site lighting/camera calibration); governance burden (audit retention, disputes, liability wording).

---
## 6. Ethics, Governance, and Risk Note
(Detailed: `Appendix/Risk_Note/risk_note.md`.)

### Main Risks
1. **Label mismatch:** screening categories ≠ AGMARK grades → misrepresentation if shown as official.
2. **Non-representative data:** lighting/camera/variety shift → silent accuracy loss.
3. **Automation bias:** graders rubber-stamp "Accept".
4. **Bias in labels:** model scales existing grader/site bias.
5. **Privacy/commercial confidentiality:** farmer/lot data in logs.

### Harm Scenarios
Farmer underpaid when a good lot is mis-screened and a lazy reviewer accepts the downgrade; exporter shipment rejected after a missed defect; grader blamed for AI-influenced decision; farmers from one region systematically disadvantaged if labels reflect regional bias.

### Controls
Guardrails: no auto-certification; banner "screening only"; hard-review on defect class/poor image. Review: blind mode + seeded errors. Escalation: senior grader queue. Logging: immutable audit log (timestamp, model version, image hash, prediction, confidence, route, human decision, reason).

### Launch Blockers
FA-rate on `Defective` above ceiling; no expert validation of categories; demonstrated bias across sites/varieties; inability to explain/log decisions; any stakeholder using output as official grade.

---
## 7. Prototype / MVP
See `04_Prototype_Readme.md`. **Type:** Streamlit web app (+ training script). **Demonstrates:** image → prediction → confidence routing → override with reason → audit log. **Not yet proven:** real-world accuracy, cross-site generalisation, AGMARK equivalence, ERP integration, economics at scale.

## 8. AI Collaboration & Contribution
See `05_AI_Collaboration_Log.md`, `06_Individual_Contribution_Statement.md`, `Appendix/AI_Build_and_SelfEvaluation.md`.

## 9. Final Recommendation
**Prototype → controlled paid pilot (gated).** Iterate if G1 not met; **stop** if model fails to beat the rules baseline. Do not scale until G3.
