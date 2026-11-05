# PRD-lite — CardamomLens v0.1
**Owner:** Avinash Kumar Jha · **Status:** Draft for Milestone 2 · **Decision supported:** first-pass quality screening of small cardamom samples

## 1. Problem & goals
Manual visual grading is slow, inconsistent and unrecorded. **Goals:** (G1) reduce full-inspection load on clear samples; (G2) standardise first-pass screening; (G3) create an audit trail. **Non-goals:** issuing official AGMARK grades, setting prices, replacing graders, farmer-facing features, ERP integration.

## 2. Users & stories
| Persona | Story | Error cost |
|---|---|---|
| Auction-centre grader | As a grader, I want a fast second opinion with confidence so I can focus on borderline lots. | Wrong accept → farmer/buyer dispute |
| Export QA officer | As QA, I want a documented pre-shipment screen so I can catch defect risk before dispatch. | Missed defect → shipment rejection (very high) |
| Senior grader / supervisor | As supervisor, I want to review overrides and disagreements to calibrate the team. | Drift unnoticed |

## 3. Functional requirements
| ID | Requirement | Priority |
|---|---|---|
| F1 | Upload JPG/PNG of a sample | P0 |
| F2 | Image-quality gate (too dark/blurry → "retake") | P1 |
| F3 | Predict category ∈ {Premium, Standard, Low_Grade, Defective} with probability | P0 |
| F4 | Route: Accept-for-screening vs Manual review using **configurable** thresholds | P0 |
| F5 | Force Manual review if class = Defective, top-2 margin < δ, or quality gate fails | P0 |
| F6 | Show Grad-CAM heatmap | P1 |
| F7 | Human decision capture: confirm/override, **reason mandatory on override or review** | P0 |
| F8 | Append-only audit log (time, user, image hash, model version, probs, route, decision, reason) | P0 |
| F9 | Persistent banner: "Screening category — not an official AGMARK grade" | P0 |
| F10 | Blind mode: reviewer records own category before seeing AI output (sampled %) | P1 |
| F11 | Lot-level PDF screening report | P1 (revenue) |

## 4. AI-specific requirements (what changes vs a normal PRD)
- **Data assumptions:** see Final Report §4; public data likely unrepresentative → pilot-collected validation set mandatory.
- **Confidence semantics:** calibrated (temperature scaling); thresholds chosen on validation set to meet FA-rate ceiling, *not* by default 0.5.
- **Fallback logic:** model missing/error → manual-only mode; never silent default to Accept.
- **Versioning:** every prediction logs model version + threshold config hash.
- **Drift:** weekly review of confidence distribution and override rate per site.

## 5. Acceptance criteria (MVP)
- Given a high-confidence non-defect prediction → app shows Accept-for-screening and logs on confirm.
- Given a Defective prediction at any confidence → Manual review forced; Accept disabled.
- Given an override → cannot save without reason.
- Given no model file → app runs in DEMO mode with visible warning.
- Routing function covered by unit tests.

## 6. Rollout & rollback
Shadow mode (AI hidden, logged) → assisted mode → blind-sample mode. Rollback: toggle to manual-only via config; no data loss.

## 7. Open questions
Exact class definitions from domain expert; permitted imaging setup at sites; data-sharing consent; liability wording; who owns the labels.
