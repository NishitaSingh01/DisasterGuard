# 11 --- Implementation Roadmap

## 1. Purpose

This roadmap proposes a safe implementation order after the design
documents are reviewed. It is not a claim that any phase has already
been completed.

## 2. Implementation phases

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
## 3. Implementation Priority

Build the reliable reporting and tracking foundation before introducing advanced AI capabilities. A successfully saved report must remain available even when AI services are unavailable.

Recommended priority order:

1. Data model and backend foundation.
2. Authentication and access control.
3. Public reporting and secure tracking.
4. Authority dashboard and interactive map.
5. Background AI processing and analysis.
6. Related-report detection, incident grouping and independent statuses.
7. Resource allocation assistance and analytics refinements.
8. Security testing, quality assurance and deployment.

The implementation order may be adjusted when necessary, provided that system dependencies, security requirements and data integrity are maintained.

**Important:** Report persistence must be established before AI processing is introduced. AI service failures must never result in the loss of submitted reports.
## 4. Risk Register

| Risk | Potential Impact | Mitigation Strategy |
|---|---|---|
| AI service unavailable | AI analysis delayed or unavailable | Persist reports before AI processing; maintain AI processing states and retry failed analysis. |
| Background job or queue failure | Reports remain unprocessed | Monitor job status, log failures, and implement retry mechanisms. Ensure report data remains safely stored. |
| False report grouping | Incorrect incident relationships and operational confusion | Use confidence thresholds, authority review for uncertain matches, and allow authorized corrections. |
| False AI classification | Misleading classification or severity suggestions | Display confidence and uncertainty; require authority review before operational decisions. |
| Tracking token exposure | Unauthorized access to citizen tracking information | Store tokens as secure hashes, use HTTPS, rate-limit tracking requests, and never log raw tokens. |
| Invalid location data | Incorrect map placement | Validate geographic coordinates on the server and handle missing or invalid location data. |
| Database failure or data loss | Loss of submitted reports and system disruption | Use database backups, appropriate access controls, error handling, and recovery procedures. |
| Unauthorized access or modifications | Compromised data and loss of trust | Enforce authentication, role-based access control, server-side authorization, and audit logging. |
| Incorrect resource recommendations | Inefficient or inappropriate resource allocation | Treat recommendations as advisory and require authorized human approval before allocation. |
| Poor AI dataset | Weak model performance and unreliable predictions | Evaluate using appropriate metrics, use representative test data, and document model limitations. |
| Scope expansion | Delayed completion and increased complexity | Follow the approved project scope, prioritize essential features, and document future enhancements separately. |

## 5. Definition of Done

### 5.1 Feature-Level Definition of Done

A feature is considered complete only when:

- Its approved functional requirements are implemented.
- Input validation and appropriate error states are handled.
- Authentication and backend authorization are applied where required.
- Relevant unit, integration, or end-to-end tests pass.
- Data integrity and security requirements are maintained.
- User-facing behaviour is understandable and usable.
- Relevant documentation is updated.
- The feature has been reviewed and verified against its acceptance criteria.

### 5.2 Project-Level Definition of Done

DisasterGuard is considered complete when:

- All approved core functional requirements are implemented.
- Citizens can submit reports and securely track their submissions.
- Authorities can authenticate, review, filter, and manage reports.
- The interactive disaster map displays report information correctly.
- AI classification, severity estimation, and priority scoring work as advisory features.
- Background AI processing supports appropriate failure handling and retries.
- Related-report detection and incident grouping work with confidence-based review and authority corrections.
- Individual report statuses and incident group statuses are maintained independently.
- Group membership history and important administrative actions are recorded in the audit log.
- Resource allocation recommendations require authorized human approval.
- Security, privacy, and role-based access controls are verified.
- Critical test cases pass and no unresolved critical defects remain.
- AI evaluation results, limitations, and known system constraints are documented.
- Deployment or demonstration setup instructions are available.
- Final project documentation is reviewed and updated.