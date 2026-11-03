# 09 --- Testing and Quality Plan

## 1. Testing objectives

Testing will verify that DisasterGuard: - Accepts and stores valid
public reports. - Protects private tracking information. - Enforces
authority/admin permissions. - Preserves individual reports when
grouping. - Maintains independent report, group and AI statuses. -
Handles AI failures without losing reports. - Produces useful and
reviewable AI recommendations. - Records important actions.

## 2. Testing levels

### Unit testing

Test individual functions and modules: - Report field validation. -
Coordinate validation. - Tracking-token generation and verification. -
Status-transition validation. - Priority-score calculation. -
Similarity/matching functions. - Role and permission checks.

### Integration testing

Test interactions between: - React frontend and Express API. - Express
API and MongoDB. - Express backend and FastAPI AI service. - Report
persistence and AI job initiation. - Tracking endpoint and token-hash
verification. - Status updates and audit logging.

### System testing

Test complete workflows: - Public report submission. - Secure
tracking. - Authority review. - AI analysis and re-analysis. -
Related-report grouping. - Group and individual status updates. -
Resource suggestion review.

### Security testing

-   Unauthenticated access to protected routes.
-   Citizen attempts to access another report.
-   Invalid/guessed tracking tokens.
-   Excessive tracking/login attempts.
-   Malformed request payloads.
-   Unsafe or oversized uploads.
-   Role escalation attempts.

### Usability testing

-   Can a citizen understand the form?
-   Are errors actionable?
-   Can a citizen find and use the tracking page?
-   Can an authority find a report using filters?
-   Are AI suggestions distinguishable from confirmed decisions?

## 3. Core test scenarios

  -------------------------------------------------------------------------------
  Test ID                 Scenario                        Expected result
  ----------------------- ------------------------------- -----------------------
  TC-01                   Submit report with all valid    Report is saved and
                          required fields                 credentials are issued

  TC-02                   Submit without required         Validation error; no
                          description/category/location   false success

  TC-03                   Submit invalid                  Request rejected
                          latitude/longitude              

  TC-04                   Submit unsupported/oversized    Upload rejected safely
                          image                           

  TC-05                   Track with valid Report ID and  Own report's current
                          token                           status displayed

  TC-06                   Track with wrong token          Generic access error;
                                                          no report data

  TC-07                   Track using Report ID only      Access denied

  TC-08                   Open authority API without      Unauthorized response
                          authentication                  

  TC-09                   Citizen attempts authority      Forbidden response
                          operation                       

  TC-10                   AI service fails after report   Report remains saved;
                          save                            AI state becomes Failed
                                                          or retryable

  TC-11                   AI analysis succeeds            Results linked to
                                                          correct report; state
                                                          Completed

  TC-12                   Authority requests re-analysis  New analysis attempt is
                                                          recorded

  TC-13                   Two related reports are grouped Both individual records
                                                          remain; common group ID
                                                          is set

  TC-14                   Group status changes            Individual statuses
                                                          remain unchanged

  TC-15                   Individual report status        Group status remains
                          changes                         unchanged

  TC-16                   Authority updates status        Audit event is recorded

  TC-17                   Authority reviews resource      Approval/rejection is
                          suggestion                      recorded

  TC-18                   Low-confidence AI               Uncertainty is visible;
                          classification                  report is not
                                                          automatically
                                                          deprioritized
  -------------------------------------------------------------------------------

## 4. AI evaluation

### Classification

Possible metrics: - Accuracy. - Precision. - Recall. - F1-score. -
Confusion matrix.

Use a held-out evaluation set where possible. Report class imbalance and
dataset limitations.

### Severity estimation

Evaluate against labelled examples or documented expert-assigned
categories. Clearly describe the limitations of the labels and
estimation method.

### Priority scoring

Check that the score is reproducible for the same inputs and that its
factors/weights are documented. Test edge cases and ensure low model
confidence does not automatically lower priority.

### Related-report grouping

Evaluate: - Precision of suggested matches. - Recall of known related
reports. - False grouping rate. - Missed related-report rate.

Thresholds must be selected using validation data and reviewed by
humans. Do not claim perfect duplicate detection.

## 5. Non-functional testing

-   Performance: measure report submission, dashboard load and filter
    response times.
-   Reliability: simulate AI downtime and failed uploads.
-   Security: test authentication, authorization and tracking-token
    protection.
-   Accessibility: check keyboard navigation, contrast and form labels.
-   Maintainability: review module boundaries, error handling and code
    quality.

## 6. Defect severity

-   **Critical:** data exposure, unauthorized access, loss/corruption of
    reports.
-   **High:** core reporting or tracking workflow fails.
-   **Medium:** important dashboard, map, grouping or AI workflow issue.
-   **Low:** cosmetic issue or minor usability problem.

## 7. Release readiness checklist

-   All critical workflows pass.
-   No known critical security issue remains.
-   Report persistence works independently of AI.
-   Tracking credentials are protected.
-   Grouping preserves original reports.
-   Group and individual statuses remain independent.
-   Audit events are recorded for important actions.
-   AI limitations are documented in the interface and project report.
