# 10 --- Workflows and Diagrams

## 1. Public report submission and hybrid AI processing

``` mermaid
flowchart TD
    A[Citizen opens report form] --> B[Enter category, description, location and optional image]
    B --> C[Backend validates input]
    C --> D{Valid?}
    D -->|No| E[Return validation error]
    D -->|Yes| F[Persist individual report]
    F --> G[Generate Report ID and secret tracking token]
    F --> H[Set AI status Pending]
    H --> I[Start AI analysis]
    I --> J[Classification]
    I --> K[Severity estimation]
    I --> L[Priority scoring]
    I --> M[Related-report detection]
    J --> N[Save analysis attempt]
    K --> N
    L --> N
    M --> N
    N --> O[Update AI processing status]
    G --> P[Show submission confirmation and credentials]
```

## 2. Secure report tracking

``` mermaid
flowchart TD
    A[Citizen opens tracking page] --> B[Enter Report ID and secret token]
    B --> C[Backend finds report and verifies token hash]
    C --> D{Credentials valid?}
    D -->|No| E[Generic access error]
    D -->|Yes| F[Return permitted fields]
    F --> G[Display current individual report status only]
```

## 3. Authority review

``` mermaid
flowchart TD
    A[Authority opens dashboard] --> B[Authenticate]
    B --> C{Authorized?}
    C -->|No| D[Reject access]
    C -->|Yes| E[Load dashboard]
    E --> F[Search/filter reports]
    F --> G[Open report or incident group]
    G --> H[Review report and AI suggestions]
    H --> I{Re-analysis needed?}
    I -->|Yes| J[Request re-analysis]
    J --> H
    I -->|No| K[Authority makes decision]
    K --> L[Update report or group status]
    L --> M[Record audit event]
```

## 4. Related-report grouping

``` mermaid
flowchart TD
    A[New report persisted] --> B[Find candidate related reports]
    B --> C{Suitable existing group?}
    C -->|No| D[Create new Incident Group]
    C -->|Yes| E[Select candidate group]
    D --> F[Link individual report to group]
    E --> F
    F --> G[Preserve individual Report ID and content]
    G --> H[Authority reviews group membership]
```

## 5. Independent statuses

``` mermaid
flowchart LR
    A[Authority action] --> B[Update individual report status]
    A --> C[Update incident-group status]
    B --> D[Report status stored]
    C --> E[Group status stored]
    D --> F[Audit event]
    E --> F
    G[AI service] --> H[AI processing status]
```

The three status tracks---individual report, incident group and AI
processing---are separate.

## 6. Conceptual system architecture

``` mermaid
flowchart TB
    subgraph Clients
        C[Citizen browser]
        A[Authority/Admin browser]
    end
    subgraph Application
        FE[React + Vite + Tailwind]
        API[Node.js + Express REST API]
    end
    DB[(MongoDB)]
    AI[Python FastAPI AI service]
    C --> FE
    A --> FE
    FE --> API
    API --> DB
    API --> AI
    AI --> API
    API --> DB
```
