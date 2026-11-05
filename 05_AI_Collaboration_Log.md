## AI Collaboration Log

> **Edit before submission:** this draft was prepared with Claude from the approval form and assignment packet. Keep only what is true for you; add your own prompts, corrections and examples.

### Tools Used
- **Claude** — structuring the package from the approval form; drafting PRD, roadmap, risk note; prototype scaffolding; self-evaluation.
- *(add any other: ChatGPT / Gemini / Copilot / Cursor …)*

### Where AI Helped
- Discovery/framing: converting the approval form into JTBD, opportunity matrix, RICE.
- Drafting: PRD-lite, metrics tree, roadmap, risk register, swimlane PlantUML.
- Wireframing: ASCII screens as starting points.
- Coding: Streamlit app, routing module, Grad-CAM, training script, unit tests.
- Risk review: automation bias / label-mismatch controls, blind-mode idea.

### Example Prompts or Workflows
- Prompt 1: "Given my approval form and the course packet, create the full capstone submission in markdown with a GitHub folder structure."
- Prompt 2: "Give the PM artifacts to an AI, ask it to build the product and self-evaluate: ambiguity, hard-coded vs configurable." (see `Appendix/AI_Build_and_SelfEvaluation.md`)
- Prompt 3: "Propose an initial swimlane workflow in PlantUML." 

### Where AI Was Weak or Wrong
- **Invented-looking numbers:** AI proposed unit-economics and pricing figures with no evidence → all marked *(A)* assumptions.
- **Generic thresholds:** 0.85 / 2% FA / +10 pts are not derived from data.
- **Domain gaps:** AI cannot validate AGMARK category definitions or real auction practice (e.g., sample size, lighting, procedure).
- **Self-evaluation bias:** AI reviewing its own work is agreeable → second independent AI run required.
- **Demo classifier is not AI:** a heuristic was needed to make the demo run; must not be presented as model accuracy.

### Human Corrections *(Avinash to confirm/edit)*
- Reframed "grade" → "quality-screening category" everywhere to avoid implying official AGMARK grading.
- Chose export-QA as the paying wedge because grader-minutes alone is a thin value case.
- Made defect-class always route to human regardless of confidence.
- Added "stop" criteria (model ≈ rules baseline).
- Replaced any claim of accuracy with explicit "not yet proven".

### Final Reflection
AI accelerated structuring, drafting and boilerplate code. It could not replace: opportunity choice (wedge), risk acceptance (FA ceiling), launch recommendation (gated pilot), monetisation judgment, and governance claims — those were decided by the PM and need validation with real graders and exporters.
