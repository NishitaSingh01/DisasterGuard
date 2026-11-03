# 04 --- Database Design

## 1. Database choice

MongoDB is the selected database. The following is a conceptual
collection design. Field types, indexes, validation rules and
embedded-versus-referenced choices should be finalized during
implementation.

## 2. Main collections

### USERS

Stores authority and administrator accounts.

  Field             Purpose
  ----------------- ------------------------
  `_id`             Unique user identifier
  `name`            Display name
  `email`           Login identity; unique
  `passwordHash`    Secure password hash
  `role`            `AUTHORITY` or `ADMIN`
  `accountStatus`   Active, disabled, etc.
  `createdAt`       Account creation time
  `updatedAt`       Last update time

Public citizens do not need accounts for the current reporting workflow.

### INCIDENT_REPORTS

Stores every individual citizen submission.

  -----------------------------------------------------------------------
  Field                               Purpose
  ----------------------------------- -----------------------------------
  `_id`                               Internal database identifier

  `reportId`                          Public unique Report ID

  `groupId`                           Reference to an Incident Group

  `category`                          Citizen-provided or confirmed
                                      category

  `description`                       Incident description

  `latitude`                          Validated latitude

  `longitude`                         Validated longitude

  `imageReference`                    Optional stored image reference

  `reportStatus`                      Submitted, Acknowledged, In
                                      Progress, Resolved

  `trackingTokenHash`                 Hash of secret tracking token

  `aiProcessingStatus`                Pending, Processing, Completed,
                                      Failed

  `submittedAt`                       Submission timestamp

  `updatedAt`                         Last update timestamp

  `createdByUserId`                   Nullable; public reports may have
                                      no account user
  -----------------------------------------------------------------------

Do not store a raw tracking token. Return it to the citizen only at the
time of generation, using a secure delivery/display flow.

### INCIDENT_GROUPS

Represents a potentially shared real-world incident.

  Field            Purpose
  ---------------- -----------------------------------------------------
  `_id`            Internal identifier
  `groupId`        Unique Incident Group ID
  `incidentType`   Group's current incident category
  `groupStatus`    Submitted, Acknowledged, In Progress, Resolved
  `createdAt`      Group creation timestamp
  `updatedAt`      Last update timestamp
  `createdBy`      System or authorized actor reference, as applicable

A group can contain one or more reports. Each report belongs to one
group in the initial model.

### AI_ANALYSES

Stores analysis attempts separately so re-analysis does not erase
earlier outputs.

  Field                 Purpose
  --------------------- ----------------------------------------
  `_id`                 Analysis identifier
  `reportId`            Related report
  `modelVersion`        Model/version identifier
  `predictedCategory`   Suggested category
  `confidence`          Model confidence, where available
  `severityEstimate`    Estimated severity
  `priorityScore`       Calculated priority
  `relatedReportIds`    Candidate related reports
  `processingStatus`    Pending, Processing, Completed, Failed
  `createdAt`           Analysis time
  `reviewedBy`          Optional authority user reference
  `reviewOutcome`       Optional correction/review outcome

### RESOURCES

Stores resource inventory or resource availability information.

  Field                  Purpose
  ---------------------- --------------------------------
  `_id`                  Resource identifier
  `name`                 Resource name
  `resourceType`         Category/type
  `quantityAvailable`    Available quantity
  `location`             Resource location, if relevant
  `availabilityStatus`   Availability state
  `updatedAt`            Last update time

### RESOURCE_SUGGESTIONS

Stores suggested resource allocations pending human review.

  Field                 Purpose
  --------------------- -----------------------------
  `_id`                 Suggestion identifier
  `groupId`             Related incident group
  `resourceId`          Suggested resource
  `suggestedQuantity`   Suggested quantity
  `reason`              Explanation
  `approvalStatus`      Pending, Approved, Rejected
  `reviewedBy`          Authority user
  `reviewedAt`          Review timestamp

### AUDIT_LOGS

Stores important actions.

  Field           Purpose
  --------------- -------------------------------------------------
  `_id`           Audit event identifier
  `actorUserId`   User who performed the action, if authenticated
  `action`        Action name
  `targetType`    Report, group, user, resource, etc.
  `targetId`      Target record identifier
  `timestamp`     Event time
  `details`       Minimal structured change details

Avoid storing secrets or unnecessary personal information in audit
details.

## 3. Relationships

``` mermaid
erDiagram
    INCIDENT_GROUPS ||--|{ INCIDENT_REPORTS : contains
    INCIDENT_REPORTS ||--o{ AI_ANALYSES : has
    INCIDENT_GROUPS ||--o{ RESOURCE_SUGGESTIONS : receives
    RESOURCES ||--o{ RESOURCE_SUGGESTIONS : referenced_by
    USERS ||--o{ AUDIT_LOGS : performs
    USERS ||--o{ RESOURCE_SUGGESTIONS : reviews
```

## 4. Status separation

Three different status concepts must remain independent:

-   `reportStatus`: progress of an individual citizen report.
-   `groupStatus`: overall response progress for the incident group.
-   `aiProcessingStatus`: progress of automated analysis.

Changing one must not silently change the others.

## 5. Index recommendations

Proposed indexes: - Unique index on `reportId`. - Unique index on
`groupId`. - Unique index on user `email`. - Index on `groupId` in
`INCIDENT_REPORTS`. - Indexes on `reportStatus`, `groupStatus` and
`submittedAt` for dashboard filtering. - Index on `reportId` and
`createdAt` in `AI_ANALYSES`. - Geospatial index for report coordinates
if using MongoDB geospatial queries.

## 6. Data integrity rules

-   Preserve each original report when grouping.
-   Never use a group ID as a replacement for a report ID.
-   Validate coordinates before persistence.
-   Validate uploaded file type and size.
-   Store tracking-token hashes, not raw tokens.
-   Keep AI analysis attempts distinguishable by timestamp/model
    version.
-   Use consistent timestamps (UTC storage is recommended).
-   Record actor and timestamp for important administrative changes.

## 7. Open schema decisions

-   Whether images are stored in object storage or a database-managed
    file service.
-   Exact location representation and geospatial query strategy.
-   Whether group membership needs a separate association collection if
    future many-to-many or group-history requirements arise.
-   Exact audit retention period.
-   Whether citizen contact details are collected at all; avoid
    collecting them unless a defined requirement justifies it.
