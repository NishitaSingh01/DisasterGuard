# DisasterGuard --- Project Documentation

**Project type:** Academic software engineering / AI-assisted disaster
reporting prototype\
**Documentation version:** 1.0 (design baseline)\
**Status:** Design and documentation phase --- implementation has not
started

## What is DisasterGuard?

DisasterGuard is an AI-assisted disaster incident reporting and
management system. Citizens can submit reports without creating
accounts. Authorized response personnel use a protected dashboard to
review reports, map incidents, assess AI recommendations, manage
statuses, and record resource information.

AI supports decision-making; it does not replace human authority or
emergency services.

## Documentation index

  ---------------------------------------------------------------------------
  File                                    Purpose
  --------------------------------------- -----------------------------------
  `docs/01_Project_Overview.md`           Problem, goals, scope, users and
                                          boundaries

  `docs/02_Requirements.md`               Functional/non-functional
                                          requirements and acceptance
                                          criteria

  `docs/03_System_Architecture.md`        High-level architecture and
                                          component responsibilities

  `docs/04_Database_Design.md`            Conceptual data model and
                                          collections

  `docs/05_Module_Design.md`              Module responsibilities and
                                          workflows

  `docs/06_API_Design.md`                 Proposed REST API contract

  `docs/07_UI_UX_Design.md`               Screens, navigation and interaction
                                          principles

  `docs/08_Security_and_Privacy.md`       Authentication, authorization,
                                          privacy and safeguards

  `docs/09_Testing_and_Quality_Plan.md`   Test strategy and acceptance
                                          scenarios

  `docs/10_Workflows_and_Diagrams.md`     Main system workflows in Mermaid

  `docs/11_Implementation_Roadmap.md`     Suggested implementation sequence
                                          and milestones

  `docs/12_Decision_Register.md`          Confirmed decisions, assumptions
                                          and open decisions

  `docs/diagrams/`                        Standalone architecture and
                                          workflow diagrams
  ---------------------------------------------------------------------------

## Technology stack

-   **Frontend:** React, Vite, Tailwind CSS, React Router, Leaflet,
    Recharts
-   **Backend:** Node.js, Express, REST APIs
-   **Database:** MongoDB
-   **AI service:** Python, FastAPI, scikit-learn, Pandas; Sentence
    Transformers optional
-   **Version control:** Git and GitHub
-   **Deployment direction:** Hosted frontend and backend with managed
    MongoDB; provider selection is not fixed here

## Important design principles

1.  Preserve every valid citizen submission as an individual report.
2.  Use a separate incident group to connect reports believed to
    describe the same real-world incident.
3.  Keep individual report status separate from incident-group status.
4.  Keep AI processing status separate from both incident statuses.
5.  AI output is advisory and subject to human review.
6.  A failed AI analysis must not erase or invalidate a saved report.
7.  A citizen must provide both their Report ID and secret tracking
    token to see their own current report status.
8.  Public-facing tracking must not expose another citizen's report or
    private information.

## Document conventions

-   "Shall" indicates a required system behaviour.
-   "Should" indicates a recommended behaviour.
-   "Proposed" identifies a design recommendation that must be validated
    during implementation planning.
-   Thresholds, deployment providers and certain operational policies
    remain open until explicitly selected.

## Current project status

The project is in the design/documentation phase. These documents
establish the baseline for review. They do not claim that any feature
has already been implemented, tested or deployed.
