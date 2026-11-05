# CardamomLens — AI-Assisted Quality-Screening Copilot for Small Cardamom Grading

**Course:** MBA ZG583 — Management of AI Products (Capstone)
**Instructor:** Prof. Vinaya Sathyanarayana
**Team:** CardamomLens (Team_26074) — *individual project*
**Repo:** https://github.com/MAIP2026July/Team_26074

## Team member details

| Name | Role | GitHub ID | Email |
|---|---|---|---|
| Avinash Kumar Jha | Sole member — all roles (strategy, PM artifacts, UX, data/eval, build, risk) | akvit2006  | 2025mb26074@wilp.bits-pilani.ac.in |

## One-line pitch
A decision-support copilot for auction-centre graders: upload a cardamom sample photo → get a quality-screening category, confidence, Grad-CAM explanation and a routed action (**Accept / Manual review**). The human always owns the official AGMARK grade, and every override is logged.

## Recommendation (summary)
**Prototype → paid, tightly controlled pilot with an export QA team.** Do **not** scale until label-validity against AGMARK and false-accept rates are proven. See `01_Executive_Summary.md`.

## Repository map (as per submission instructions)

```
Team_26074/
├── README.md                          ← you are here
├── 01_Executive_Summary.md
├── 02_Final_Report.md                 ← strategy, UX, data/model, business, ethics (all 7 sections)
├── 03_Presentation_Outline.md         ← slide-by-slide script for the review pitch
├── 04_Prototype_Readme.md             ← what the prototype shows / does not prove
├── 05_AI_Collaboration_Log.md
├── 06_Individual_Contribution_Statement.md
├── Appendix/
│   ├── PRD/PRD.md
│   ├── Wireframes/wireframes.md       ← ASCII screens
│   ├── Metrics/metrics.md             ← metric tree + go/no-go thresholds
│   ├── Roadmap/roadmap.md             ← MVP → first revenue roadmap
│   ├── Risk_Note/risk_note.md
│   ├── Workflow/swimlane.puml         ← PlantUML swimlane (render at plantuml.com)
│   ├── AI_Build_and_SelfEvaluation.md ← "give artifacts to AI: build + self-evaluate" step
│   └── Screenshots_or_Video/          ← add screenshots after running the app
└── prototype/                         ← Streamlit app, routing logic, training script, tests
```

## Quick start (prototype)
```bash
cd prototype
pip install -r requirements.txt
streamlit run app.py
pytest tests/        # routing-logic tests
```
Without a trained model the app runs in **DEMO mode** (a colour/darkness heuristic that doubles as the *rules baseline*). See `04_Prototype_Readme.md`.

## Honesty note
All numbers marked *(assumption)* are planning assumptions to be validated in interviews/pilot — none are measured results. No accuracy claim is made for the prototype.
