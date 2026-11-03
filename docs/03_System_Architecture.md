# 03 --- System Architecture

## 1. Architecture overview

DisasterGuard uses a layered client-server architecture with a separate
AI service.

### Selected technology stack

  Layer                          Technology
  ------------------------------ -----------------------
  Frontend                       React + Vite
  Styling                        Tailwind CSS
  Frontend routing               React Router
  Interactive map                Leaflet
  Charts and analytics           Recharts
  Main backend                   Node.js + Express
  API style                      REST
  Database                       MongoDB
  AI service                     Python + FastAPI
  AI/data libraries              scikit-learn, Pandas
  Optional semantic similarity   Sentence Transformers
  Version control                Git + GitHub

## 2. High-level components

### Frontend

-   Citizen report form.
-   Secure report tracking page.
-   Authority login.
-   Authority dashboard.
-   Incident list and detail views.
-   Interactive incident map.
-   Resource and analytics screens.
-   Administrator user-management screens.

### Express backend

-   Public report submission and validation.
-   Authentication and role-based authorization.
-   Report and incident-group management.
-   Tracking-token verification.
-   Dashboard and analytics endpoints.
-   Audit logging.
-   Communication with the AI service.

### MongoDB

Stores users, incident reports, incident groups, AI analysis results,
resource records, tracking-token hashes and audit events.

### FastAPI AI service

-   Disaster category classification.
-   Severity estimation.
-   Priority scoring.
-   Related-report similarity analysis.
-   Model evaluation support.

## 3. Component interaction

``` mermaid
flowchart LR
    Citizen[Citizen Browser] --> Frontend[React Frontend]
    Authority[Authority/Admin Browser] --> Frontend
    Frontend --> API[Express REST API]
    API --> DB[(MongoDB)]
    API --> AI[FastAPI AI Service]
    AI --> DB
    API --> Audit[Audit Events]
    Audit --> DB
```

The browser communicates with the Express API. The Express backend is
the main authority for validation, access control and data changes. The
AI service provides analysis results but does not directly make
operational decisions.

## 4. Hybrid AI processing

After the backend validates and saves a report, it initiates AI
processing. The report should already exist before AI analysis begins.

Recommended logical sequence:

1.  Persist report and tracking-token hash.
2.  Set AI processing status to `Pending`.
3.  Initiate an AI analysis job.
4.  Set status to `Processing`.
5.  Receive classification, severity, priority and related-report
    results.
6.  Save the results against the report and update status to
    `Completed`.
7.  If processing fails, record `Failed` and allow retry.
8.  Authorized personnel may request re-analysis.

A background-job mechanism is recommended so that slow AI processing
does not block report submission. The specific queue/job technology is
not selected in this design.

## 5. Trust boundaries

-   Public browser to Express API: untrusted input; validate all fields
    and uploads.
-   Authority browser to Express API: authenticate and authorize every
    protected operation.
-   Express API to MongoDB: backend-controlled data access.
-   Express API to AI service: validate requests and returned data;
    treat model output as advisory.
-   Public tracking endpoint: return only the minimum permitted fields
    after Report ID and token verification.

## 6. Deployment direction

The intended deployment shape is: - Hosted React frontend. - Hosted
Express backend. - Hosted FastAPI AI service, or an appropriately
isolated deployment alongside the backend. - Managed MongoDB database.

Specific hosting providers, domain names, secrets management and
production infrastructure are not selected in this document.

## 7. Architecture quality attributes

-   **Modularity:** frontend, API, database access and AI service have
    distinct responsibilities.
-   **Security:** authentication and authorization are enforced by the
    backend.
-   **Reliability:** report persistence is independent of AI success.
-   **Maintainability:** REST endpoints and service modules have clear
    boundaries.
-   **Explainability:** priority scoring factors and AI outputs should
    be reviewable.
-   **Scalability:** AI analysis can be moved to background workers as
    processing needs grow.

## 8. Architecture decisions still to be detailed

-   Background-job/queue implementation.
-   Exact AI model and training/evaluation dataset.
-   Priority scoring formula and weights.
-   Related-report similarity thresholds.
-   Hosting provider and deployment configuration.
-   Notification mechanism, if notifications are later added.
