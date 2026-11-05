## Executive Summary — CardamomLens

### Problem
Small cardamom is graded at auction centres and pre-shipment largely by **manual visual inspection**. It is slow at peak volumes, **inconsistent between graders and sites**, leaves **no digital record** of how a grade was decided, and depends on alertness to catch subtle defects (mould, thrips/insect damage). Grades drive farmer payment and export eligibility; missed defects can cause costly shipment rejections in strict markets (e.g., EU, Saudi Arabia), and contested grades cannot be independently reviewed.

### Target User / Customer
- **Primary user:** auction-centre graders and field officers (manual visual grading today).
- **Primary paying customer (pilot):** export QA teams / exporters and large aggregators doing pre-shipment checks (faster buying cycle than government bodies).
- **Indirect beneficiary:** farmers (fairer, defensible grading) — not a direct user.

### Product Concept
A web-based copilot. The grader uploads a sample photo and receives: (1) a predicted **quality-screening category** (explicitly *not* an AGMARK grade), (2) a **confidence score**, (3) a **Grad-CAM heatmap** showing what drove the prediction, and (4) a **recommended action: Accept-for-screening or Manual review**. Low-confidence, boundary, or defect-flagged samples are *always* routed to a human. Overrides require a reason and everything lands in an audit log.

### Why AI?
Fixed rules (size/colour thresholds) fail on the *combination* of subtle cues — surface texture, colour variation, partial defects — and break across lighting, cameras and pod varieties. A transfer-learned image classifier learns these patterns from labelled examples. We will **prove this** by benchmarking against a rules baseline; if the baseline is within noise of the model, we stop.

### Recommendation
**Prototype now; proceed to a small paid pilot (1 export-QA site or 1 aggregator) only if** (a) the model clearly beats the rules baseline on held-out data, (b) the false-accept rate on defective samples is below the pre-agreed ceiling, and (c) an expert validates the mapping between screening categories and AGMARK. **Do not scale** before label validity and cross-site generalisation are shown. Human remains the final authority for official grade throughout.
