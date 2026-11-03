# 08 --- Security and Privacy

## 1. Security objectives

DisasterGuard handles incident locations, descriptions, images and
operational information. It must protect private information, prevent
unauthorized changes and maintain the integrity of reports.

## 2. Authentication

-   Authority and administrator users must authenticate before accessing
    protected functions.
-   Passwords must never be stored in plaintext.
-   Use a recognized password-hashing algorithm with appropriate
    parameters.
-   Use secure session or token handling.
-   Provide logout and account-disable behaviour.
-   Public citizens do not need accounts for report submission or
    tracking.

## 3. Role-based access control

### Public citizen

-   Submit a report.
-   Track a report only with its Report ID and secret token.
-   Cannot access authority dashboards, internal notes, all-report lists
    or audit logs.

### Authority

-   Access authorized operational dashboard features.
-   Review and manage reports and groups.
-   Update statuses.
-   Review AI outputs and resource suggestions.
-   Access only functions granted by the assigned role.

### Administrator

-   Manage authorized accounts and roles.
-   Review audit logs.
-   Perform administrative configuration and oversight.

Every protected API operation must enforce permissions on the backend.
Hiding a button in the frontend is not sufficient.

## 4. Secure report tracking

-   Generate a high-entropy secret token for each report.
-   Store only a secure hash of the token.
-   Require both Report ID and token for tracking.
-   Never treat a Report ID as an authentication credential.
-   Do not log the raw token.
-   Apply rate limiting to tracking attempts.
-   Return generic errors for invalid credentials.
-   Show only the current individual report status and permitted basic
    fields.

## 5. Input and upload security

-   Validate all required fields server-side.
-   Validate latitude and longitude ranges.
-   Apply length limits to text fields.
-   Sanitize or safely render user-provided text.
-   Restrict upload size and accepted image formats.
-   Verify file content rather than relying only on filename extension.
-   Store uploads outside executable application paths.
-   Generate safe server-side file names.
-   Reject unexpected or unsafe file content.

## 6. Privacy

-   Collect only information needed for incident reporting.
-   Avoid collecting citizen identity/contact details unless a
    documented requirement is approved.
-   Do not expose one citizen's report to another citizen.
-   Do not expose internal notes or sensitive operational information
    through public tracking.
-   Keep public responses separate from authority responses.
-   Define data retention and deletion policies before deployment.

## 7. API and infrastructure security

-   Use HTTPS in deployment.
-   Keep database credentials, signing keys and service secrets in
    environment variables or a secrets manager.
-   Never commit `.env` files or secrets to Git.
-   Apply rate limiting to sensitive public endpoints.
-   Restrict CORS to trusted frontend origins in deployment.
-   Restrict AI service access to trusted backend communication.
-   Keep dependencies updated and review vulnerabilities.
-   Use least-privilege database credentials.

## 8. Audit logging

Record important events such as: - Authority login/logout events where
appropriate. - Report status changes. - Incident-group status changes. -
Group membership corrections. - AI feedback/re-analysis requests. -
Resource approvals/rejections. - Account and role changes.

Each audit event should identify the actor (where authenticated),
action, target and timestamp. Do not store passwords, raw tracking
tokens or unnecessary sensitive payloads in logs.

## 9. AI safety and human oversight

-   AI outputs must be labelled as suggestions or estimates.
-   Authorities retain final decision-making power.
-   Low confidence must not automatically mean low priority.
-   Keep model version and analysis time for traceability.
-   Preserve the original report if AI fails.
-   Do not automatically retrain or deploy models based solely on
    authority feedback.
-   Resource suggestions require human approval.
-   Similarity scores must not be treated as proof that two reports are
    the same incident.

## 10. Operational disclaimer

DisasterGuard is an academic decision-support prototype. It is not an
emergency hotline, official warning service or replacement for emergency
services. The interface should not imply that submitting a report
guarantees a response.
