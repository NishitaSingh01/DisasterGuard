# 11 --- Implementation Roadmap

## 1. Purpose

This roadmap proposes a safe implementation order after the design
documents are reviewed. It is not a claim that any phase has already
been completed.

## 2. Suggested phases

### Phase 0 --- Documentation and design approval

-   Review project scope and exclusions.
-   Approve requirements and acceptance criteria.
-   Review architecture and database model.
-   Finalize open decisions in the decision register.
-   Confirm API contracts and screen wireframes.

**Exit condition:** design baseline approved before coding.

### Phase 1 --- Project foundation

-   Create repository structure.
-   Set up React/Vite frontend.
-   Set up Express backend.
-   Connect MongoDB.
-   Configure environment variables and Git ignore rules.
-   Add consistent error handling and basic logging.

**Exit condition:** frontend can communicate with backend and backend
can persist a test record.

### Phase 2 --- Authentication and roles

-   Implement authority/admin login.
-   Implement secure password hashing and session/token handling.
-   Add backend role middleware.
-   Add administrator account management.

**Exit condition:** protected endpoints reject unauthenticated and
unauthorized requests.

### Phase 3 --- Citizen reporting and tracking

-   Build report form.
-   Add server-side validation and optional image upload.
-   Persist reports.
-   Generate Report ID and secret tracking token.
-   Store token hash.
-   Build current-status tracking page.

**Exit condition:** end-to-end public submission and secure tracking
work.

### Phase 4 --- Authority dashboard and map

-   Build dashboard layout.
-   Add report list, search and filters.
-   Add incident detail view.
-   Add Leaflet map and marker interactions.
-   Add dashboard summaries/charts.

**Exit condition:** authority can review and locate reports.

### Phase 5 --- AI service

-   Build FastAPI service.
-   Implement baseline classification.
-   Implement documented severity and priority logic.
-   Implement related-report candidate detection.
-   Add processing states and failure handling.
-   Connect Express orchestration to AI service.
-   Add authority-triggered re-analysis and feedback.

**Exit condition:** reports persist independently and AI outputs are
saved and reviewable.

### Phase 6 --- Incident grouping and status management

-   Implement incident groups.
-   Link related reports without deleting individual records.
-   Add group detail view.
-   Implement independent group and report statuses.
-   Add group-membership review/correction according to approved
    permissions.
-   Add audit events.

**Exit condition:** grouping and dual-status workflows pass integration
tests.

### Phase 7 --- Resource assistance

-   Implement resource inventory.
-   Display resource suggestions.
-   Add approval/rejection workflow.
-   Record decisions in audit log.

**Exit condition:** resource suggestions require human approval.

### Phase 8 --- Testing, hardening and deployment

-   Complete functional and integration tests.
-   Run security and usability checks.
-   Evaluate AI models and document limitations.
-   Configure deployment and secrets.
-   Prepare demo data and project presentation.
-   Complete final report and user documentation.

**Exit condition:** acceptance criteria pass and prototype is ready for
demonstration.

## 3. Suggested implementation priority

Build the reliable reporting and tracking foundation before AI
sophistication. A saved report must remain available even when AI is
unavailable.

Recommended order: 1. Data model and backend foundation. 2. Public
reporting and secure tracking. 3. Authentication and authority
dashboard. 4. Map and filtering. 5. AI processing. 6. Grouping and dual
statuses. 7. Resources and analytics refinements. 8. Testing and
deployment.

## 4. Risk register

  -----------------------------------------------------------------------
  Risk                    Impact                  Mitigation
  ----------------------- ----------------------- -----------------------
  AI service unavailable  AI analysis delayed     Persist reports first;
                                                  retry analysis

  False report grouping   Incorrect operational   Human review and
                          view                    correction

  False AI classification Misleading suggestion   Display uncertainty;
                                                  authority review

  Tracking token exposure Unauthorized status     Hash tokens, rate-limit
                          access                  and avoid logging

  Invalid location data   Incorrect map placement Server-side coordinate
                                                  validation

  Scope expansion         Delayed completion      Enforce project
                                                  exclusions

  Poor AI dataset         Weak model quality      Evaluate honestly and
                                                  document limitations

  Unauthorized status     Loss of trust/data      RBAC and audit logging
  changes                 integrity               
  -----------------------------------------------------------------------

## 5. Definition of done

A feature is complete only when: - Its requirements are implemented. -
Validation and error states are handled. - Backend authorization is
applied where needed. - Relevant tests pass. - User-facing behaviour is
understandable. - Documentation is updated.
