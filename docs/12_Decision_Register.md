# 12 --- Decision Register

This document records decisions that shape the DisasterGuard design.

## 1. Confirmed decisions

  -----------------------------------------------------------------------
  ID                      Topic                   Decision
  ----------------------- ----------------------- -----------------------
  DR-01                   Project scope           Eight core feature
                                                  areas plus supporting
                                                  authentication, audit
                                                  and AI feedback

  DR-02                   Frontend                React + Vite + Tailwind
                                                  CSS

  DR-03                   Frontend routing        React Router

  DR-04                   Map                     Leaflet

  DR-05                   Charts                  Recharts

  DR-06                   Main backend            Node.js + Express

  DR-07                   API style               REST

  DR-08                   Database                MongoDB

  DR-09                   AI service              Python + FastAPI

  DR-10                   AI libraries            scikit-learn and
                                                  Pandas; Sentence
                                                  Transformers optional

  DR-11                   Citizen access          Citizens can submit
                                                  without logging in

  DR-12                   Staff access            Authority and
                                                  administrator login
                                                  required

  DR-13                   Tracking                Hybrid private tracking
                                                  using Report ID +
                                                  secret token

  DR-14                   Tracking display        Current status only; no
                                                  full status history on
                                                  citizen tracking page

  DR-15                   AI workflow             Automatic analysis
                                                  after report save, with
                                                  authority-triggered
                                                  re-analysis

  DR-16                   AI failure              Report remains saved if
                                                  AI processing fails

  DR-17                   Duplicate handling      Group related reports
                                                  under a common Incident
                                                  Group ID

  DR-18                   Report preservation     Every citizen
                                                  submission retains its
                                                  own Report ID and
                                                  record

  DR-19                   Status model            Maintain both
                                                  individual report
                                                  status and
                                                  incident-group status

  DR-20                   Status independence     Group and individual
                                                  statuses do not
                                                  automatically overwrite
                                                  each other

  DR-21                   Individual statuses     Submitted,
                                                  Acknowledged, In
                                                  Progress, Resolved

  DR-22                   Group statuses          Submitted,
                                                  Acknowledged, In
                                                  Progress, Resolved

  DR-23                   AI processing status    Pending, Processing,
                                                  Completed, Failed;
                                                  separate from incident
                                                  statuses

  DR-24                   AI authority            AI is advisory;
                                                  authorities retain
                                                  final decisions

  DR-25                   Resource suggestions    Human review/approval
                                                  required

  DR-26                   UI theme                Dark sidebar with light
                                                  content

  DR-27                   UI layout               Balanced layout and
                                                  balanced density

  DR-28                   Map visibility          Current baseline places
                                                  the map in the
                                                  protected authority
                                                  experience; no public
                                                  map is assumed
  -----------------------------------------------------------------------

## 2. Confirmed project exclusions

-   No automatic emergency dispatch.
-   No autonomous rescue operations.
-   No automatic physical resource deployment.
-   No guaranteed disaster prediction.
-   No government-system integration in the current scope.
-   No production native mobile application in the current scope.
-   No automatic deletion of reports identified as similar.

## 3. Open decisions requiring approval

  --------------------------------------------------------------------------
  ID                Topic             Current proposal     Status
  ----------------- ----------------- -------------------- -----------------
  OD-01             Incorrect group   Authorities can      Awaiting user
                    membership        correct normal       approval
                                      grouping mistakes;   
                                      administrators       
                                      handle exceptional   
                                      corrections and      
                                      disputes             

  OD-02             Related-report    Tune location, time, To be designed
                    thresholds        category and         
                                      text-similarity      
                                      thresholds using     
                                      validation examples  

  OD-03             Grouping          Define whether       To be finalized
                    confirmation      system-created       
                                      groups are           
                                      immediately active   
                                      or marked for        
                                      authority review     

  OD-04             Priority formula  Define factors,      To be designed
                                      weights and score    
                                      bands                

  OD-05             Severity scale    Define levels and    To be designed
                                      evidence criteria    

  OD-06             Upload storage    Select storage       To be finalized
                                      approach for images  

  OD-07             Background jobs   Select queue/job     To be finalized
                                      mechanism for AI     
                                      processing           

  OD-08             Hosting provider  Select deployment    To be finalized
                                      services             

  OD-09             Data retention    Define               To be finalized
                                      retention/deletion   
                                      rules                

  OD-10             Exact API schemas Finalize             During
                                      request/response     implementation
                                      schemas and          planning
                                      validation           
  --------------------------------------------------------------------------

## 4. Notes

The documentation can be used as a design baseline while open decisions
are reviewed. Open decisions must not be presented as already approved
requirements.
