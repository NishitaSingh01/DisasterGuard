# 02 --- Software Requirements Specification

**Document status:** Design baseline v1.0\
**System:** DisasterGuard

## 1. Purpose

This document defines the functional and non-functional requirements for
DisasterGuard. It provides a basis for architecture, implementation,
acceptance testing and project evaluation.

## 2. Scope

DisasterGuard supports public incident reporting, authority-side report
management, map-based incident viewing, AI-assisted classification and
triage, related-report grouping, secure status tracking, resource
assistance and audit logging.

The system is an academic decision-support prototype. It does not
autonomously dispatch emergency services or deploy physical resources.

## 3. User roles and access summary

  -------------------------------------------------------------------------
  Capability         Citizen / public          Authority      Administrator
  ---------------- ------------------ ------------------ ------------------
  Submit a report       Yes, no login   Not required for   Not required for
                                             public form        public form

  Track own report   Yes, Report ID +     Not applicable     Not applicable
                         secret token                    

  View authority                   No                Yes     Administrative
  dashboard                                                       access as
                                                                  permitted

  View/manage all                  No    Yes, subject to    Yes, subject to
  reports                                    permissions     administrative
                                                                       need

  Update                           No                Yes      As authorized
  report/group                                           
  status                                                 

  Review/correct                   No                Yes      As authorized
  AI output                                              

  Manage authority                 No         No, unless                Yes
  accounts                                  specifically 
                                               delegated 

  Review audit                     No        Limited, if                Yes
  logs                                         permitted 
  -------------------------------------------------------------------------

The backend---not merely the frontend---must enforce permissions.

## 4. Functional requirements

### Authentication and access

  -----------------------------------------------------------------------
  ID                                  Requirement
  ----------------------------------- -----------------------------------
  FR-01                               The system shall authenticate
                                      authority and administrator users
                                      before granting access to protected
                                      dashboard functions. Public
                                      citizens shall be able to submit
                                      reports without creating an
                                      account.

  FR-02                               The system shall support role-based
                                      permissions for public users,
                                      authority users and administrators.
                                      The backend shall enforce
                                      permissions for protected
                                      operations.
  -----------------------------------------------------------------------

### Citizen reporting and validation

  -----------------------------------------------------------------------
  ID                                  Requirement
  ----------------------------------- -----------------------------------
  FR-03                               The system shall allow a citizen to
                                      submit a disaster category,
                                      description, location and optional
                                      image.

  FR-04                               The system shall validate required
                                      fields, coordinate ranges and
                                      uploaded-file constraints before
                                      accepting a report.

  FR-05                               The system shall allow authorized
                                      personnel to view, search, filter
                                      and manage incident reports.

  FR-06                               The system shall display incident
                                      locations as map markers, with
                                      visual indicators for relevant
                                      severity or status information.
                                      Selecting a marker shall open
                                      permitted incident details.
  -----------------------------------------------------------------------

### AI-assisted analysis

  -----------------------------------------------------------------------
  ID                                  Requirement
  ----------------------------------- -----------------------------------
  FR-07                               The system shall use report text to
                                      suggest a likely disaster category
                                      and shall communicate uncertainty
                                      where available.

  FR-08                               The system shall estimate incident
                                      severity using available text and
                                      structured evidence according to
                                      documented criteria.

  FR-09                               The system shall calculate an
                                      incident priority score using
                                      defined urgency factors and a
                                      reproducible scoring method.

  FR-10                               The system shall identify
                                      potentially related or duplicate
                                      reports for grouping and authority
                                      review.

  FR-15                               The system shall allow authorized
                                      authorities to correct AI
                                      predictions and save feedback for
                                      model evaluation.

  FR-17                               After a valid report is saved, the
                                      system shall automatically initiate
                                      AI analysis for classification,
                                      severity estimation, priority
                                      scoring and related-report
                                      detection.

  FR-18                               The system shall allow authorized
                                      authorities to request AI
                                      re-analysis when appropriate.

  FR-19                               If AI processing fails, the system
                                      shall preserve the submitted
                                      report, record the AI processing
                                      state and allow a retry. AI failure
                                      shall not invalidate the report.
  -----------------------------------------------------------------------

### Dashboard, analytics and status tracking

  -----------------------------------------------------------------------
  ID                                  Requirement
  ----------------------------------- -----------------------------------
  FR-11                               The authority dashboard shall
                                      present incident counts, category
                                      distributions and report-status
                                      summaries, with relevant filters.

  FR-12                               The system shall support the
                                      individual report statuses
                                      Submitted, Acknowledged, In
                                      Progress and Resolved.

  FR-16                               The system shall generate a unique
                                      Report ID and secret tracking token
                                      for each successfully submitted
                                      report. A citizen shall provide
                                      both credentials to view the
                                      current status of that report.

  FR-22                               The system shall maintain a
                                      separate status for each incident
                                      group.

  FR-23                               The system shall maintain an
                                      independent status for every
                                      individual incident report.

  FR-24                               The system shall allow authorized
                                      authorities to update group and
                                      individual report statuses
                                      independently.

  FR-25                               The system shall record important
                                      status changes and administrative
                                      actions in an audit log.
  -----------------------------------------------------------------------

### Related-report grouping

  -----------------------------------------------------------------------
  ID                                  Requirement
  ----------------------------------- -----------------------------------
  FR-20                               The system shall group reports
                                      believed to describe the same
                                      real-world incident under a common
                                      Incident Group ID while preserving
                                      each individual report and its
                                      unique Report ID.

  FR-21                               The system shall allow authorized
                                      authorities to review and manage
                                      incident-group associations.
  -----------------------------------------------------------------------

### Resource assistance

  -----------------------------------------------------------------------
  ID                                  Requirement
  ----------------------------------- -----------------------------------
  FR-13                               The system shall allow authorized
                                      personnel to record available
                                      resources and review suggested
                                      resource allocations.

  FR-14                               The system shall record important
                                      status changes and administrative
                                      actions with an actor and
                                      timestamp.

  FR-26                               Authorized personnel shall review
                                      and approve resource suggestions
                                      before they are treated as
                                      decisions.
  -----------------------------------------------------------------------

**Note:** FR-14 and FR-25 overlap by design in this baseline. FR-14
covers the general audit capability; FR-25 emphasizes status-change
auditing. They can be consolidated during implementation planning if a
single audit requirement is preferred.

## 5. Non-functional requirements

  -----------------------------------------------------------------------
  ID                                  Requirement
  ----------------------------------- -----------------------------------
  NFR-01 Security                     Passwords shall be stored using a
                                      suitable password-hashing
                                      algorithm. Protected APIs shall
                                      require authentication and
                                      authorization.

  NFR-02 Data integrity               Each report shall retain a stable
                                      unique identifier. Grouping shall
                                      not delete or overwrite original
                                      submissions.

  NFR-03 Reliability                  A valid report shall remain saved
                                      even if AI processing or an
                                      external dependency fails.

  NFR-04 Performance                  Common dashboard and report
                                      operations should complete within a
                                      reasonable target established
                                      during testing.

  NFR-05 Usability                    Citizen submission and report
                                      tracking shall be understandable
                                      with clear validation and error
                                      messages.

  NFR-06 Maintainability              Frontend, backend, database access
                                      and AI processing shall be
                                      separated into maintainable
                                      modules/services.

  NFR-07 Auditability                 Important administrative and
                                      status-changing actions shall be
                                      traceable to an authorized actor
                                      and timestamp.

  NFR-08 Privacy                      Public-facing responses shall not
                                      expose private citizen information
                                      or another citizen's report
                                      details.

  NFR-09 Accessibility                Interfaces should use readable
                                      contrast, labels,
                                      keyboard-accessible controls and
                                      responsive layouts.

  NFR-10 AI evaluation                AI performance shall be evaluated
                                      using appropriate test data and
                                      documented metrics; model output
                                      shall remain advisory.
  -----------------------------------------------------------------------

## 6. Status model

### Individual report status

-   **Submitted:** Report has been received and stored.
-   **Acknowledged:** An authority has acknowledged the report.
-   **In Progress:** Investigation or response is underway.
-   **Resolved:** The individual report has been marked resolved.

### Incident-group status

The same four status labels are used for the overall incident group.
Group status is maintained independently from each member report's
status.

### AI processing status

AI processing has its own independent state: - Pending - Processing -
Completed - Failed

A report can be `Submitted` while AI processing is `Processing`. AI
processing status must never be confused with incident response status.

## 7. Business rules

1.  A public citizen does not need an account to submit a report.
2.  A successful report submission receives a unique Report ID and a
    secret tracking token.
3.  The secret token shall not be stored in plaintext; store a secure
    hash and verify the supplied token.
4.  Report ID alone shall not authorize private tracking access.
5.  Every citizen submission remains an individual report, even when
    linked to an incident group.
6.  A report shall be linked to one incident group at a time in the
    initial data model.
7.  A group may contain one or more reports; a new group is created when
    no suitable existing group is found.
8.  Related-report matching is probabilistic and must not silently
    discard reports.
9.  Group and individual report statuses are independent.
10. AI outputs are recommendations. Authorized personnel retain final
    decision-making authority.
11. Low AI confidence shall not automatically mean low incident
    priority.
12. AI failure shall not prevent a valid report from being saved.
13. Resource suggestions require human review and approval.
14. Public tracking displays only the current status of the citizen's
    own report.

## 8. Acceptance criteria

### Citizen submission

-   A citizen can submit without logging in.
-   Missing required fields and invalid coordinates are rejected with
    understandable messages.
-   Optional images are checked against file type and size limits.
-   A valid report is persisted and receives a Report ID and tracking
    token.
-   The citizen receives confirmation and the tracking credentials.

### Secure tracking

-   Both Report ID and secret token are required.
-   Incorrect credentials do not disclose report details.
-   A valid citizen sees only the current status of their own report.
-   Tracking responses do not disclose internal notes, other reports or
    private citizen information.

### AI processing

-   AI processing starts automatically after report persistence.
-   The system records processing state and associates outputs with the
    correct report.
-   A failure leaves the report available to authorities and permits
    retry.
-   Authorized personnel can request re-analysis.
-   AI outputs can be reviewed and corrected by authorized personnel.

### Grouping and statuses

-   Related reports can be linked to a common Incident Group ID.
-   Original Report IDs and report records remain intact.
-   Each report and each group has its own status.
-   Group status changes do not silently overwrite member report
    statuses.
-   Authorities can review group membership.
-   Status changes are auditable.

### Authority dashboard and resources

-   Unauthenticated users cannot access protected dashboard APIs.
-   Authorities can search and filter reports.
-   Authorities can view permitted AI results and group relationships.
-   Resource suggestions are not treated as decisions until approved by
    authorized personnel.

## 9. Failure and exception requirements

-   If AI service is unavailable, preserve the report and mark analysis
    as failed or pending retry.
-   If image upload fails, provide a clear error and do not silently
    claim the image was attached.
-   If database persistence fails, do not issue a success confirmation
    or valid tracking credentials.
-   If a tracking credential is invalid, return a generic access error
    that does not reveal whether a Report ID exists.
-   If two authorities update the same record concurrently, the backend
    should prevent silent overwriting and communicate conflicts.
-   If a report is incorrectly grouped, the system must provide an
    authorized correction process. Exact role permissions for
    unlinking/reassignment remain an open decision in
    `12_Decision_Register.md`.

## 10. Out of scope

Autonomous dispatch, autonomous rescue operations, automatic physical
resource deployment, guaranteed disaster prediction and
government-system integration are excluded from this project baseline.
