# 11 --- Implementation Roadmap

## 1. Purpose

This roadmap proposes a safe implementation order after the design
documents are reviewed. It is not a claim that any phase has already
been completed.

## 2. Suggested phases

### Phase 0 --- Documentation and design approval

- Review and finalize project scope, objectives and exclusions.
- Review functional and non-functional requirements.
- Verify that requirements have clear acceptance criteria.
- Review system architecture and database design.
- Finalize API contracts and module responsibilities.
- Review UI/UX designs and user workflows.
- Review security, privacy and role-based access control decisions.
- Review testing strategy and quality requirements.
- Verify that workflow diagrams match the finalized system design.
- Finalize unresolved decisions in the decision register.
- Check consistency across all project documentation.
- Approve the complete design baseline before implementation begins.

**Exit condition:** All design documents are reviewed and approved, critical design decisions are finalized, and the project is ready to move into implementation.


### Phase 1 --- Project foundation

- Create and configure the frontend project using React and Vite.
- Configure Tailwind CSS and React Router.
- Create and configure the backend project using Node.js and Express.
- Set up the Python environment for the AI service using FastAPI.
- Configure MongoDB connectivity.
- Establish the initial frontend, backend and AI service folder structures.
- Configure environment variables and example environment files.
- Add `.gitignore` rules to prevent sensitive files and credentials from being committed.
- Configure Git branches and a basic development workflow.
- Add initial application health-check endpoints.
- Document local development and setup instructions.

**Exit condition:** The frontend, backend and AI service environments are configured, the database connection is verified, and the basic application structure is ready for feature development.

### Phase 2 --- Authentication and access control

- Implement secure authority and administrator login.
- Implement password hashing and secure credential storage.
- Implement role-based access control for authority and administrator roles.
- Protect restricted backend API endpoints using authentication and authorization.
- Implement secure session or token management.
- Add login rate limiting and protection against repeated failed attempts.
- Implement session expiration and logout handling.
- Implement public citizen reporting without requiring account creation.
- Implement secure citizen tracking using Report ID and secret tracking token.
- Ensure that Report ID alone cannot grant access to tracking information.
- Restrict public tracking responses to explicitly permitted information.
- Test unauthorized access attempts and role-based permissions.

**Exit condition:** Authority and administrator authentication, backend authorization and secure public tracking are implemented and tested. Citizens can submit reports without creating accounts.

### Phase 3 --- Citizen reporting and tracking

- Build the public citizen incident reporting form.
- Implement required-field validation and server-side input validation.
- Support optional incident image uploads with file type and size restrictions.
- Validate geographical coordinates before saving reports.
- Persist each valid incident report as an independent database record.
- Generate a unique Report ID for every successfully submitted report.
- Generate a secure secret tracking token for citizen access.
- Store the tracking token securely as a hash rather than storing the raw token.
- Display submission confirmation and tracking credentials to the citizen.
- Build a public report tracking page requiring both Report ID and secret tracking token.
- Display only explicitly permitted public tracking information.
- Implement generic error responses for invalid tracking credentials.
- Protect tracking endpoints against repeated unauthorized attempts.
- Ensure that citizens cannot access another citizen's report or internal authority information.
- Verify that report submission succeeds independently of AI service availability.

**Exit condition:** Citizens can submit valid reports without creating accounts, receive unique tracking credentials and securely view permitted information about their own reports. Submitted reports remain saved even if AI services are unavailable.

### Phase 4 --- Authority dashboard and interactive map

- Build the authority dashboard layout according to the approved UI/UX design.
- Display incident statistics and summary information.
- Implement an incident report listing with pagination.
- Add filtering and searching by incident category, severity, priority and status.
- Implement individual incident report detail views.
- Integrate an interactive map using Leaflet.
- Display incident locations using map markers.
- Use distinct marker styles to represent incident severity and status.
- Implement marker selection to view relevant incident information.
- Synchronize map filters with the incident report listing.
- Display individual report and incident-group statuses separately.
- Display AI processing states where available.
- Apply role-based permissions to dashboard operations.
- Implement loading, empty, error and confirmation states.
- Ensure the dashboard is responsive and accessible.

**Exit condition:** Authorized users can view, search, filter and inspect incident reports through the dashboard and interactive map. Report statuses, group statuses and AI processing states are displayed distinctly.

### Phase 5 --- AI integration and analysis

- Set up the AI service using Python and FastAPI.
- Implement the incident classification module.
- Implement severity estimation based on incident information.
- Implement priority scoring using documented factors and rules.
- Prepare and preprocess suitable datasets for AI model development.
- Train and evaluate AI models using appropriate evaluation metrics.
- Implement confidence scores and uncertainty handling where applicable.
- Integrate the AI service with the backend API.
- Implement background AI task processing through a job queue and worker.
- Maintain AI processing states: Pending, Processing, Completed and Failed.
- Store AI-generated results and associate them with the correct incident report.
- Implement error handling and retry policies for failed AI tasks.
- Allow authorized authorities to request AI re-analysis.
- Display AI predictions and recommendations in the authority dashboard.
- Ensure AI outputs remain advisory and subject to human review.
- Document model limitations, evaluation results and known failure cases.

**Exit condition:** The AI service is integrated with the backend and can perform incident classification, severity estimation and priority scoring through background processing. AI results are stored, evaluated and made available for authorized human review without affecting report persistence.

### Phase 6 --- Duplicate detection and incident grouping

- Implement related-report detection using incident descriptions, geographical locations, categories and other relevant information.
- Calculate similarity and confidence scores for potentially related reports.
- Define and configure thresholds for automatic grouping and authority review.
- Implement automatic grouping for eligible high-confidence matches.
- Create new incident groups when no appropriate existing group is available.
- Send uncertain grouping suggestions to the authority review queue.
- Build an interface for authorities to review suggested groupings.
- Allow authorized authorities to confirm, reject or correct suggested groupings.
- Allow authorities to add, remove or move reports between incident groups.
- Allow administrators to handle exceptional group membership corrections.
- Maintain separate statuses for individual reports and incident groups.
- Preserve individual report records, unique Report IDs and original submission details.
- Maintain current group membership and historical membership changes.
- Record the affected report, previous group ID, new group ID, actor, timestamp and reason for every membership change.
- Record grouping decisions and membership modifications in the audit log.
- Test grouping accuracy, false matches and missed related reports.

**Exit condition:** The system can identify potentially related reports, automatically group eligible high-confidence matches and send uncertain matches for authority review. Authorized users can correct group membership while preserving individual reports, independent statuses and complete membership history.

### Phase 7 --- Resource allocation assistance

- Define the types of emergency resources that the system will support.
- Identify the information required to generate resource allocation suggestions.
- Develop a rule-based or data-driven resource recommendation mechanism.
- Use incident severity, priority, location and resource requirements as relevant input factors.
- Generate resource allocation suggestions for authorities.
- Display suggested resource allocations in the authority dashboard.
- Allow authorized authorities to review allocation suggestions.
- Implement approval, rejection and modification workflows.
- Record the reasons for rejection or modification.
- Maintain allocation records and their current status.
- Record important resource allocation decisions in the audit log.
- Ensure that AI-generated suggestions cannot independently authorize or execute resource allocation.

**Exit condition:** The system can generate and display resource allocation suggestions, and authorized authorities can review, approve, reject or modify them. All important allocation decisions are recorded and remain under human control.

### Phase 8 --- Security, testing and quality assurance

- Perform unit testing for frontend, backend and AI service components.
- Perform integration testing across the frontend, backend, database and AI service.
- Perform end-to-end testing of major citizen and authority workflows.
- Test authentication and role-based authorization.
- Test public tracking access using valid and invalid credentials.
- Verify that citizens cannot access other citizens' reports or internal authority information.
- Test report submission, validation and data persistence.
- Test AI processing states, background execution, failure handling and retry mechanisms.
- Evaluate AI classification, severity estimation, priority scoring and related-report detection.
- Test automatic grouping and authority-assisted membership corrections.
- Verify that individual reports and incident-group statuses remain independent.
- Test audit logging and protection against unauthorized modification.
- Test resource allocation approval, rejection and modification workflows.
- Perform security testing for common application vulnerabilities.
- Perform performance and reliability testing under expected workloads.
- Test accessibility, responsiveness and usability.
- Document defects, test results and corrective actions.
- Retest corrected defects and perform regression testing.

**Exit condition:** Critical workflows pass their tests, security and privacy requirements are verified, AI limitations are documented, and no unresolved critical defects remain.
### Phase 9 --- Deployment and documentation

- Prepare production configuration for the frontend, backend and AI service.
- Select suitable hosting and deployment platforms.
- Configure production environment variables and secrets securely.
- Configure the production database and access restrictions.
- Set up HTTPS and secure communication between deployed services.
- Deploy the frontend application.
- Deploy the backend API.
- Deploy the AI service and background processing components.
- Configure database backups and recovery procedures.
- Implement application logging and basic system monitoring.
- Verify deployed API endpoints and service health checks.
- Perform end-to-end testing in the deployment environment.
- Update installation, setup and deployment documentation.
- Document known limitations, operational requirements and future improvements.
- Prepare the final project report and demonstration.

**Exit condition:** All required application components are deployed or prepared for the intended demonstration environment, deployment procedures are documented, and the complete system passes final verification.
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
