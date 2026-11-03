# 05 --- Module Design

## 1. Frontend modules

### Citizen reporting module

Responsibilities: - Display public incident form. - Collect category,
description, coordinates/location and optional image. - Validate user
input before submission. - Show confirmation and tracking credentials
after successful persistence.

### Citizen tracking module

Responsibilities: - Accept Report ID and secret tracking token. - Submit
credentials to the tracking API. - Display only the current status and
permitted basic report information. - Show a generic error for invalid
credentials.

### Authentication module

Responsibilities: - Provide authority/admin login. - Maintain
authenticated session/token safely. - Protect frontend routes for user
experience. - Never rely on frontend route protection as the only
security control.

### Authority dashboard module

Responsibilities: - Display report summaries and analytics. - Provide
search, filters and sorting. - Link to report details and incident
groups. - Show AI processing state separately from incident status.

### Incident map module

Responsibilities: - Display incident markers using Leaflet. - Use
suitable marker styling for status/severity. - Open permitted
report/group details when a marker is selected. - Avoid exposing private
citizen details.

### Resource module

Responsibilities: - Display resource inventory. - Record resource
availability. - Show resource suggestions. - Capture human approval or
rejection.

### Administrator module

Responsibilities: - Manage authority accounts and roles. - Review audit
records. - Provide administrative configuration where in scope.

## 2. Backend modules

### Authentication and authorization

-   Verify credentials.
-   Hash passwords using a suitable password-hashing algorithm.
-   Issue and validate secure sessions/tokens.
-   Enforce role and permission checks on every protected route.

### Report service

-   Validate report fields and coordinates.
-   Validate optional image metadata and file limits.
-   Persist report before initiating AI analysis.
-   Generate Report ID and secret tracking token.
-   Store only the token hash.

### Tracking service

-   Verify Report ID and secret token.
-   Return only permitted current-status information.
-   Avoid revealing whether an ID exists when credentials are invalid.

### Incident-group service

-   Find candidate related reports.
-   Create a new group when no suitable group exists.
-   Link a report to a selected group.
-   Preserve individual reports.
-   Support authorized review of group membership.

### AI orchestration service

-   Initiate analysis after report persistence.
-   Track Pending, Processing, Completed and Failed states.
-   Store analysis attempts.
-   Support authority-triggered re-analysis.
-   Retry failures without losing the original report.

### Status management service

-   Update individual report status.
-   Update incident-group status independently.
-   Validate permitted transitions.
-   Create audit events for changes.

### Resource service

-   Maintain resource records.
-   Generate or display allocation suggestions.
-   Require human approval before a suggestion is treated as a decision.

### Audit service

-   Record important user actions and status changes.
-   Include actor, target and timestamp.
-   Prevent public users from accessing internal audit data.

## 3. AI modules

### Category classification

Input: report description and relevant structured data.\
Output: suggested category and confidence information where available.

### Severity estimation

Input: available incident text and structured evidence.\
Output: estimated severity and supporting factors where feasible.

Severity represents the seriousness or impact of an incident.

### Priority scoring

Input: defined urgency factors such as estimated severity and other
documented signals.\
Output: reproducible score or tier with an explanation.

Priority represents how urgently an incident should be reviewed or acted
upon. Low model confidence must not automatically produce a low
priority.

### Related-report detection

Input: report text, category, time and location.\
Output: candidate related reports or an existing group suggestion.

Similarity is a signal, not proof. Preserve all reports and allow
authority review.

### AI feedback and evaluation

-   Save authority corrections.
-   Track model version and analysis time.
-   Evaluate model performance using appropriate labelled data.
-   Do not automatically retrain or deploy a model solely because a
    correction was submitted.

## 4. Main end-to-end workflows

### Workflow A --- Public report submission

1.  Citizen opens report form.
2.  Citizen enters category, description and location; image is
    optional.
3.  Backend validates all fields and upload constraints.
4.  Backend persists the report.
5.  Backend generates Report ID and tracking token; stores token hash.
6.  Backend initiates AI processing.
7.  Citizen receives confirmation and credentials.

### Workflow B --- AI analysis

1.  Analysis job is initiated after report persistence.
2.  AI service performs classification, severity, priority and
    related-report analysis.
3.  Results are saved as an analysis attempt.
4.  Processing status is updated.
5.  Authority reviews results.
6.  Authority may request re-analysis or correct predictions.

### Workflow C --- Secure citizen tracking

1.  Citizen enters Report ID and secret token.
2.  Backend verifies the token hash.
3.  Backend returns permitted report information and current individual
    report status only.
4.  Invalid credentials produce a generic access error.

### Workflow D --- Authority management

1.  Authority signs in.
2.  Backend verifies identity and role.
3.  Authority views dashboard and filters reports.
4.  Authority reviews report details, AI outputs and group membership.
5.  Authority updates individual or group status.
6.  Backend records audit events.

### Workflow E --- Related-report grouping

1.  New report is saved.
2.  System searches for candidate related reports.
3.  System creates or suggests an incident group association.
4.  Original report remains independently stored.
5.  Authority reviews group membership and corrects it where authorized.

### Workflow F --- Resource assistance

1.  Authority records available resources.
2.  System presents suggestions for an incident group.
3.  Authority reviews the suggestion.
4.  Authority approves or rejects it.
5.  The decision is recorded in the audit log.
6.  No physical resource is automatically deployed by the system.
