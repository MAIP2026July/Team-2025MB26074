# Ethics, Governance and Risk Note
| # | Risk | Likelihood | Impact | Control |
|---|---|---|---|---|
| R1 | Label mismatch vs AGMARK | High | High | Never label output as official; validation study (G3); persistent banner |
| R2 | Non-representative data → silent failure | High | High | Pilot-collected validation set; per-site slices; image-quality gate; drift review |
| R3 | Automation bias | Medium | High | Confidence bands not just %, blind mode, seeded errors, forced review for defect |
| R4 | Biased labels scaled by model | Medium | High | ≥2 graders, κ reporting, subgroup (region/variety) analysis, re-label loop |
| R5 | False accept of defective lot | Medium | Very high | Conservative threshold set to FA ceiling; Defective always reviewed |
| R6 | Privacy / confidentiality (farmer, lot, site) | Medium | Medium | Minimise PII; hash images; consent; retention policy; access control |
| R7 | Misuse (output shown as certificate) | Medium | High | Watermarked "screening only" reports; contract terms |
| R8 | Over-claiming accuracy in sales | Medium | High | Report only measured, sliced results |

(Likelihood/impact ratings are judgement calls, not measured.)

## Harm scenarios
Farmer underpaid after an unchecked downgrade; exporter rejection after a missed mould; grader unfairly blamed; regional bias.

## Controls
Guardrails (no auto-certify, hard-review triggers) · Review (supervisor queue) · Escalation (senior grader, dispute process) · Logging (append-only audit with model version) · Rollback (manual-only toggle).

## Launch blockers
FA-rate above ceiling · experts reject category definitions · material cross-site/variety bias · no consent/data agreement · any customer treating output as an official grade.
