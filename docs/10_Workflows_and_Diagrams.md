# 10 --- Workflows and Diagrams

## 1. Public Report Submission and Hybrid AI Processing

```mermaid
flowchart TD
    A[Citizen opens report form] --> B[Enter incident details]
    B --> C[Backend validates report]
    C --> D{Is the report valid?}

    D -->|No| E[Return validation errors]
    D -->|Yes| F[Save individual report]

    F --> G[Generate Report ID and secret tracking token]
    G --> H[Set AI processing status to Pending]
    G --> I[Show submission confirmation and tracking credentials]

    H --> J[Add AI task to background processing queue]
    J --> K[Background worker starts processing]
    K --> L[Set AI status to Processing]

    L --> M[Perform AI analysis]
    M --> N[Classify incident]
    M --> O[Estimate severity]
    M --> P[Calculate priority score]
    M --> Q[Detect related reports]

    N --> R{Did AI processing succeed?}
    O --> R
    P --> R
    Q --> R

    R -->|Yes| S[Save AI results]
    S --> T[Set AI status to Completed]

    R -->|No| U[Log processing failure]
    U --> V[Set AI status to Failed]
    V --> W{Can the task be retried?}

    W -->|Yes| J
    W -->|No| X[Keep report saved and notify authority]
```

**Workflow notes:**

- A valid report must be saved before AI processing begins.
- The citizen receives submission confirmation without waiting for AI analysis.
- AI processing runs in the background.
- AI processing status is maintained separately from individual report and incident-group statuses.
- Failed AI tasks must not cause submitted reports to be lost.
- Failed tasks may be retried according to predefined retry rules.
- AI-generated results remain advisory and require human review before becoming confirmed operational decisions.

## 2. Secure Report Tracking

```mermaid
flowchart TD
    A[Citizen opens tracking page] --> B[Enter Report ID and secret tracking token]
    B --> C[Backend validates input]
    C --> D[Find report using Report ID]
    D --> E[Verify secret token against stored token hash]
    E --> F{Are credentials valid?}

    F -->|No| G[Return generic access error]
    F -->|Yes| H[Check citizen tracking permissions]

    H --> I[Retrieve permitted public tracking information]
    I --> J[Display individual report status]
    J --> K[Show permitted status updates]

    G --> L[Do not reveal whether Report ID or token was incorrect]
```

**Workflow notes:**

- Citizens must provide both the Report ID and its corresponding secret tracking token.
- The backend must verify the token securely without storing the raw token.
- Report ID alone must never grant access to tracking information.
- Invalid credentials must produce a generic error that does not reveal whether a report exists.
- Citizens may view only information explicitly approved for public tracking.
- Public tracking must not expose internal authority notes, private incident-group information, AI processing details or another citizen's report.
- Tracking access must be protected against repeated unauthorized attempts.

## 3. Authority Review and Management

```mermaid
flowchart TD
    A[Authority opens dashboard] --> B[Enter login credentials]
    B --> C[Backend authenticates user]
    C --> D{Authentication successful?}

    D -->|No| E[Display login error]
    D -->|Yes| F[Check authority permissions]

    F --> G{Authorized?}
    G -->|No| H[Reject access]
    G -->|Yes| I[Display authority dashboard]

    I --> J[View and filter incident reports]
    J --> K[Select individual report or incident group]
    K --> L[Review incident details and available AI results]

    L --> M{Action required?}

    M -->|Update report status| N[Update individual report status]
    M -->|Update group status| O[Update incident group status]
    M -->|Review AI result| P[Review AI recommendation]
    M -->|Review grouping| Q[Open grouping review queue]

    P --> R[Accept or override recommendation]
    Q --> S[Review related-report suggestion]
    S --> T[Confirm, reject or correct grouping]

    N --> U[Record authorized action in audit log]
    O --> U
    R --> U
    T --> U

    U --> V[Refresh dashboard]
```

**Workflow notes:**

- Only authenticated and authorized authorities may access the authority dashboard and its protected operations.
- Authorities can view, search and filter incident reports.
- AI-generated classification, severity and priority results are recommendations for human review.
- Authorities can update individual report statuses and incident-group statuses independently.
- Authorities can review uncertain related-report suggestions and correct group membership according to their permissions.
- Important status changes, AI review actions and grouping modifications must be recorded in the audit log.
- Every operation must be authorized by the backend, not merely controlled by frontend visibility.

## 4. Related-Report Detection and Incident Grouping

```mermaid
flowchart TD
    A[New incident report is saved] --> B[AI detects potentially related reports]
    B --> C[Calculate similarity and confidence score]

    C --> D{Confidence level}

    D -->|High confidence| E[Check automatic grouping rules]
    D -->|Uncertain confidence| F[Send suggestion to authority review queue]

    E --> G{Does an appropriate group exist?}
    G -->|Yes| H[Add report to existing incident group]
    G -->|No| I[Create a new incident group]

    F --> J[Authority reviews suggested reports]
    J --> K{Grouping approved?}

    K -->|Yes| L[Authority confirms group membership]
    K -->|No| M[Reject suggestion or select another group]

    H --> N[Preserve individual report record]
    I --> N
    L --> N
    M --> N

    N --> O[Update current group membership]
    O --> P[Maintain membership history]
    P --> Q[Record audit event]
```

**Grouping rules:**

- High-confidence matches may be grouped automatically according to predefined thresholds and grouping rules.
- Uncertain matches must be sent to the authority review queue.
- Authorities can confirm suggested groupings and add, remove or move reports between groups.
- Administrators can handle exceptional group membership corrections.
- Every membership change must preserve the previous group ID, new group ID, affected report, actor, timestamp and reason.
- Individual reports must retain their unique Report IDs and original report records.
- Grouping must never delete or overwrite individual reports.
- Individual report statuses and incident-group statuses must remain independent.
- All membership changes must be recorded in the audit history.

## 5. Resource Allocation Workflow

```mermaid
flowchart TD
    A[Authority reviews incident report or group] --> B[Assess incident severity and priority]
    B --> C[Identify required resources]
    C --> D[Generate resource allocation suggestion]
    D --> E[Authority reviews suggestion]
    E --> F{Is allocation approved?}

    F -->|Yes| G[Approve resource allocation]
    F -->|No| H[Reject or request modification]

    G --> I[Record approved allocation]
    H --> J[Record rejection or modification reason]

    I --> K[Update allocation status]
    J --> K

    K --> L[Record action in audit log]
    L --> M[Refresh authority dashboard]
```

**Workflow notes:**

- The system may generate resource allocation suggestions based on incident information, severity and priority.
- Resource allocation suggestions must be reviewed by an authorized authority.
- Authorities are responsible for approving or rejecting allocation suggestions.
- Rejection and modification reasons should be recorded.
- Approved allocations and important decisions must be recorded in the audit log.
- AI-generated recommendations must not independently authorize or execute operational resource allocation.

## 6. Conceptual System Architecture

```mermaid
flowchart TB

    subgraph Clients
        C[Citizen Browser]
        A[Authority / Admin Browser]
    end

    subgraph Frontend
        FE[React + Vite + Tailwind CSS]
    end

    subgraph Backend
        API[Node.js + Express REST API]
        AUTH[Authentication and Authorization]
        LOGIC[Report and Incident Management]
    end

    subgraph Background Processing
        Q[Background Job Queue]
        W[Background Worker]
    end

    subgraph AI Service
        AI[Python FastAPI AI Service]
        ML[Classification and Severity Analysis]
        DUP[Related-Report Detection]
    end

    DB[(MongoDB)]

    C --> FE
    A --> FE

    FE --> API
    API --> AUTH
    API --> LOGIC

    LOGIC --> DB
    API --> Q
    Q --> W
    W --> AI

    AI --> ML
    AI --> DUP

    ML --> W
    DUP --> W

    W --> DB
    API --> DB
```

**Architecture notes:**

- **Frontend:** Provides separate interfaces for citizens and authority/administrative users.
- **Backend API:** Handles report validation, authentication, authorization, business logic and communication between system components.
- **Authentication and Authorization:** Protects authority and administrator operations according to assigned roles and permissions.
- **Report and Incident Management:** Handles individual reports, incident groups, report statuses and group statuses.
- **Background Job Queue:** Holds AI processing tasks so that report submission does not have to wait for AI analysis.
- **Background Worker:** Executes queued tasks and communicates with the AI service.
- **AI Service:** Performs incident classification, severity estimation, priority scoring and related-report detection.
- **Database:** Stores incident reports, incident groups, user information, AI results, processing states and audit records.
- **Human Review:** AI-generated predictions and resource allocation suggestions remain advisory and are subject to authorized human review.

The background queue and worker represent conceptual components. The specific queue technology and deployment arrangement will be decided during detailed technical design.