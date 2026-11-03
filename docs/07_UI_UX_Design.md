# 07 --- UI/UX Design

## 1. Visual direction

The approved authority dashboard direction is:

-   Hybrid theme: dark sidebar with light content area.
-   Balanced layout.
-   Balanced information density.
-   Clear hierarchy between summary metrics, filters, maps, tables and
    detail panels.

The interface should be readable and functional rather than visually
crowded.

## 2. User journeys

### Citizen journey

1.  Open the public report form.
2.  Enter incident category, description and location.
3.  Optionally attach an image.
4.  Submit the report.
5.  Receive Report ID and secret tracking token.
6.  Open the tracking page and enter both credentials.
7.  View the current status of their own report.

### Authority journey

1.  Open authority login.
2.  Authenticate.
3.  View dashboard summary.
4.  Filter reports by status, category, severity, priority or date.
5.  Open report details or map marker.
6.  Review AI recommendations and related-report group.
7.  Update report or group status.
8.  Review resources and suggestions.
9.  Record decisions through system actions that generate audit events.

### Administrator journey

1.  Authenticate.
2.  Open user management or audit area.
3.  Manage authorized accounts and roles.
4.  Review important administrative actions.

## 3. Proposed screen inventory

  -----------------------------------------------------------------------
  Screen                  Main content            Key interactions
  ----------------------- ----------------------- -----------------------
  Public landing/report   Project intro and       Start report
  page                    report entry point      

  Citizen report form     Category, description,  Validate and submit
                          location, optional      
                          image                   

  Submission confirmation Report ID and secret    Copy/save credentials
                          token                   

  Citizen tracking page   Credential fields and   Verify and view status
                          current status          

  Authority login         Email and password      Sign in

  Authority dashboard     Summary cards, charts,  Filter and navigate
                          recent reports          

  Incident management     Searchable report table Search, filter, open
                                                  details

  Incident details        Report data, AI         Review and update
                          results, group          
                          information             

  Live incident map       Markers and map filters Select marker, inspect
                                                  incident

  Incident group details  Group status and        Review membership and
                          member-report summaries status

  Resource management     Resource inventory and  Add/update/review
                          suggestions             

  Analytics               Category and status     Filter reporting period
                          summaries               

  User management         Authority/admin         Create/update/disable
                          accounts                accounts

  Audit log               Action, actor, target   Filter and inspect
                          and timestamp           
  -----------------------------------------------------------------------

## 4. Authority dashboard layout

Suggested structure: - **Dark left sidebar:** Dashboard, Incidents, Live
Map, Resources, Analytics, User Management, Audit Logs, Settings. -
**Top bar:** page title, global search where appropriate, signed-in user
and logout. - **Main content:** summary cards, charts, filters and
recent incident table. - **Detail view:** report summary, location, AI
analysis, group association, status controls and audit-relevant actions.

## 5. Incident detail layout

Recommended sections: 1. Report identification and current status. 2.
Category, description, time and location. 3. Optional evidence/image
preview. 4. AI classification, confidence and model metadata where
available. 5. Severity estimate and factors. 6. Priority score and
explanation. 7. Related-report group and member summaries. 8. Authority
actions and status controls.

AI suggestions should be visually distinguished from authority-confirmed
values.

## 6. Tracking page privacy

The citizen tracking page must: - Require Report ID and secret token. -
Show only the current status of that report. - Avoid displaying other
group members' submissions. - Avoid exposing internal notes, authority
identities or private contact details. - Use a generic error for invalid
credentials.

## 7. Map interaction

-   Use Leaflet.
-   Provide useful filters.
-   Use clear marker styles for status or severity.
-   Selecting a marker opens a permitted summary/detail view.
-   Avoid showing exact private citizen information in marker popups.
-   The current baseline places the incident map inside the protected
    authority experience. A public map is not assumed.

## 8. Usability and accessibility

-   Use clear field labels and validation messages.
-   Maintain sufficient contrast.
-   Use keyboard-accessible controls.
-   Avoid communicating status using colour alone.
-   Make layouts responsive for common desktop and mobile browser sizes.
-   Provide loading, empty, success and error states.
-   Confirm consequential actions such as status changes or resource
    approvals.
