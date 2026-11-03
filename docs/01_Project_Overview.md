# 01 --- Project Overview

## 1. Project summary

**DisasterGuard** is an AI-assisted disaster incident reporting and
management system designed to help citizens submit incident information
and help authorized response personnel review, prioritize and manage
incoming reports.

Citizens can submit a report without registering or logging in. After a
successful submission, the system provides a unique Report ID and a
secret tracking token. The citizen can use both credentials to view the
current status of that report.

Authorized personnel use an authenticated authority dashboard to review
reports, view incidents on a map, examine AI recommendations, manage
incident groups and update report and group statuses.

## 2. Problem statement

During disasters, information may arrive from multiple people, in
different formats and at different times. Response personnel may need to
manually inspect reports, identify related submissions, determine which
incidents need attention and keep track of response progress.

DisasterGuard aims to organize these activities in one system and
provide AI-assisted analysis to support---not replace---human decisions.

## 3. Project objectives

-   Provide a straightforward way for citizens to submit disaster
    reports.
-   Capture useful incident details, including category, description and
    location.
-   Present reports in an authority dashboard and interactive map.
-   Suggest likely disaster categories using text classification.
-   Estimate severity and calculate a transparent priority score.
-   Identify and group reports that may describe the same real-world
    incident.
-   Allow authorities to review AI outputs and request re-analysis.
-   Track both individual report progress and overall incident-group
    progress.
-   Provide basic resource recording and allocation suggestions for
    human review.
-   Maintain an audit trail of important administrative and status
    changes.

## 4. Intended users

### Citizen / public reporter

-   Does not need an account to submit a report.
-   Provides incident information and optional image evidence.
-   Receives a Report ID and secret tracking token.
-   Can view only the current status of their own report using both
    credentials.

### Authority user

-   Signs in with an authorized account.
-   Views, searches and filters incident reports.
-   Reviews maps, AI outputs, severity, priority and possible report
    groups.
-   Updates individual report and incident-group statuses.
-   Records resource information and reviews suggestions.
-   Provides corrections to AI predictions.

### Administrator

-   Signs in with an authorized account.
-   Manages authority accounts and roles.
-   Reviews audit records and administrative settings.
-   Performs administrative oversight.

## 5. Project scope

### Included

1.  Citizen disaster reporting.
2.  Interactive incident map.
3.  Authority dashboard.
4.  AI disaster-category classification.
5.  Severity estimation and priority scoring.
6.  Individual report status tracking.
7.  Related-report detection and incident grouping.
8.  Resource assistance, including resource records and human-reviewed
    suggestions.
9.  Role-based access and audit logging.
10. AI feedback and authority-triggered re-analysis.

### Excluded from the current project

-   Automatic emergency-service dispatch.
-   Autonomous rescue or response operations.
-   Automatic physical deployment of resources.
-   Guaranteed prediction of disasters or their outcomes.
-   Direct integration with government emergency-response systems.
-   Replacing official emergency services or human incident commanders.
-   A production-grade native mobile application.
-   Treating AI predictions as final decisions.

## 6. Initial disaster categories

The initial category set is:

-   Flood
-   Fire
-   Earthquake
-   Landslide
-   Storm / severe weather
-   Other

The category list may be extended later through controlled
configuration.

## 7. Expected outcome

The expected outcome is an academic working prototype with a documented
architecture, user workflows, data model, API design, security approach
and test plan. The system should demonstrate how AI can assist incident
triage while keeping final decisions with authorized humans.

## 8. Limitations

-   AI quality depends on the quality and quantity of available
    training/evaluation data.
-   Location and text similarity may identify false matches or miss
    related reports.
-   Severity and priority estimates are approximate decision-support
    outputs.
-   The prototype is not an emergency hotline and must not be
    represented as a replacement for emergency services.
-   Actual response actions remain the responsibility of authorized
    personnel.
