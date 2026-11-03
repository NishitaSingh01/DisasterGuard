# 06 --- API Design

**API style:** REST over HTTPS\
**Main backend:** Node.js + Express\
**AI service:** Python + FastAPI

This is a proposed API contract. Exact schemas, validation libraries,
pagination format and error codes should be finalized during
implementation.

## 1. API conventions

-   Use JSON for ordinary request and response bodies.
-   Use multipart form-data for image uploads where appropriate.
-   Use HTTPS in deployed environments.
-   Validate all request bodies and query parameters.
-   Return consistent error structures.
-   Protect authority/admin endpoints with authentication and
    authorization.
-   Do not expose internal stack traces or secrets.

Suggested error shape:

``` json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "One or more fields are invalid."
  }
}
```

## 2. Public endpoints

  ------------------------------------------------------------------------
  Method                  Endpoint                 Purpose
  ----------------------- ------------------------ -----------------------
  POST                    `/api/reports`           Submit a public
                                                   incident report

  POST                    `/api/tracking/verify`   Verify Report ID and
                                                   secret token and return
                                                   permitted current
                                                   status
  ------------------------------------------------------------------------

### POST `/api/reports`

Example request fields: - `category` - `description` - `latitude` -
`longitude` - optional `image`

Successful response should include: - `reportId` - secret tracking token
(shown only at creation) - submission confirmation - current report
status

The raw tracking token must not be stored or logged.

### POST `/api/tracking/verify`

Example request:

``` json
{
  "reportId": "DG-EXAMPLE-001",
  "trackingToken": "one-time-secret-value"
}
```

Successful response should contain only permitted fields, such as: -
Report ID - category - submitted date - current individual report status

It must not include internal authority notes, other citizens' reports,
group member details or private personal information.

Invalid credentials should return a generic response.

## 3. Authentication endpoints

  -----------------------------------------------------------------------
  Method                  Endpoint                Purpose
  ----------------------- ----------------------- -----------------------
  POST                    `/api/auth/login`       Authenticate
                                                  authority/admin

  POST                    `/api/auth/logout`      End session, if using
                                                  server-managed sessions

  GET                     `/api/auth/me`          Return current
                                                  authenticated user's
                                                  permitted profile
  -----------------------------------------------------------------------

Account creation/role assignment is administrator-controlled in the
current scope; public citizen registration is not required.

## 4. Authority report endpoints

  ------------------------------------------------------------------------------------------------
  Method                  Endpoint                                         Purpose
  ----------------------- ------------------------------------------------ -----------------------
  GET                     `/api/authority/reports`                         List reports with
                                                                           filters and pagination

  GET                     `/api/authority/reports/:reportId`               View permitted report
                                                                           details

  PATCH                   `/api/authority/reports/:reportId/status`        Update individual
                                                                           report status

  POST                    `/api/authority/reports/:reportId/reanalyze`     Request AI re-analysis

  PATCH                   `/api/authority/reports/:reportId/ai-feedback`   Save
                                                                           correction/feedback
  ------------------------------------------------------------------------------------------------

Possible query filters: - `status` - `category` - `severity` -
`priority` - `groupId` - `from` - `to` - `page` - `limit`

## 5. Incident-group endpoints

  ---------------------------------------------------------------------------------------------
  Method                  Endpoint                                      Purpose
  ----------------------- --------------------------------------------- -----------------------
  GET                     `/api/authority/groups`                       List incident groups

  GET                     `/api/authority/groups/:groupId`              View group and
                                                                        permitted member-report
                                                                        summaries

  PATCH                   `/api/authority/groups/:groupId/status`       Update group status

  GET                     `/api/authority/groups/:groupId/reports`      List reports in a group

  PATCH                   `/api/authority/groups/:groupId/membership`   Proposed endpoint for
                                                                        authorized membership
                                                                        correction
  ---------------------------------------------------------------------------------------------

The exact permission policy for unlinking/reassigning reports remains
open and is recorded in the decision register.

## 6. Dashboard and map endpoints

  ---------------------------------------------------------------------------------------
  Method                  Endpoint                                Purpose
  ----------------------- --------------------------------------- -----------------------
  GET                     `/api/authority/dashboard/summary`      Counts and status
                                                                  summaries

  GET                     `/api/authority/map/incidents`          Map-ready
                                                                  incident/group markers

  GET                     `/api/authority/analytics/categories`   Category distribution

  GET                     `/api/authority/analytics/statuses`     Status distribution
  ---------------------------------------------------------------------------------------

Map responses must return only fields appropriate for authority access.
Any future public map would require a separate privacy review and
endpoint policy.

## 7. Resource endpoints

  ------------------------------------------------------------------------------------------------------------
  Method                  Endpoint                                                     Purpose
  ----------------------- ------------------------------------------------------------ -----------------------
  GET                     `/api/authority/resources`                                   List resources

  POST                    `/api/authority/resources`                                   Create resource record

  PATCH                   `/api/authority/resources/:resourceId`                       Update resource record

  GET                     `/api/authority/groups/:groupId/resource-suggestions`        View suggestions

  PATCH                   `/api/authority/resource-suggestions/:suggestionId/review`   Approve or reject
                                                                                       suggestion
  ------------------------------------------------------------------------------------------------------------

Only authorized personnel can approve suggestions. Approval records a
decision; it does not trigger automatic physical deployment.

## 8. Administrator endpoints

  Method   Endpoint                     Purpose
  -------- ---------------------------- -------------------------------
  GET      `/api/admin/users`           List authority/admin accounts
  POST     `/api/admin/users`           Create an authorized account
  PATCH    `/api/admin/users/:userId`   Update account or role
  GET      `/api/admin/audit-logs`      Review audit events

## 9. AI service interface

The Express backend should communicate with the FastAPI service through
a private service-to-service interface.

Proposed operations: - `POST /ai/analyze` --- classification, severity,
priority and related-report analysis. - `GET /ai/health` --- service
health check.

The AI service should return structured results with model/version
metadata. The Express backend remains responsible for authorization,
persistence and workflow decisions.

## 10. API security requirements

-   Authenticate all protected routes.
-   Enforce role checks in backend middleware/service logic.
-   Validate IDs and all user-controlled input.
-   Apply rate limiting to login, public report submission and tracking
    verification.
-   Use generic tracking errors.
-   Never log raw tracking tokens or passwords.
-   Limit upload size and accepted file types.
-   Restrict AI service access to trusted backend communication.
