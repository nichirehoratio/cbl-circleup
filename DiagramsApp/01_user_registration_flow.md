# Pipeline 01: User Registration

## Overview
Critical user onboarding pipeline that creates Snowflake accounts via external form submission. Implements cost-optimized data flow pattern using GitHub Actions as API proxy and Snowflake Stages as buffer layer. Handles GDPR compliance for minors and implements role-based access control.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `01` |
| **Category** | User Management & Data Capture |
| **Priority** | Very High |
| **Connects To** | `02` (Authentication), `04` (Parental Consent), `13` (Container Lifecycle) |
| **Triggered By** | YouForm webhook |
| **Outputs To** | Snowflake Users, RBAC, Email Service |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% External Layer
    A@{ shape: rounded, label: "YouForm API<br/>Data Collection" }
    B@{ shape: rounded, label: "Email Service<br/>Notifications" }
    
    %% GitHub Infrastructure Layer
    C@{ shape: rounded, label: "GitHub Actions<br/>Workflow Runner" }
    D@{ shape: cyl, label: "GitHub Secrets<br/>API Keys" }
    
    %% Snowflake Core Layer
    E@{ shape: bow-rect, label: "Snowflake Stage<br/>Raw Data Buffer" }
    F@{ shape: hex, label: "Snowflake Tasks<br/>Daily Processing" }
    G@{ shape: cyl, label: "Snowflake Users<br/>Account Management" }
    H@{ shape: diam, label: "RBAC System<br/>Permission Control" }
    
    %% Container Runtime Layer
    I@{ shape: rounded, label: "SPCS Container<br/>Streamlit App" }
    J@{ shape: doc, label: "File Storage<br/>PDF Documents" }
    
    %% Critical Flow Connections with Animation
    A e1@--> C
    C e2@--> E
    E e3@--> F
    F e4@--> G
    F e5@--> H
    G e6@--> B
    H e7@--> I
    I --> J
    D -.-> C
    
    %% Animation for Critical Path
    e1@{ animation: fast }
    e2@{ animation: fast }
    e3@{ animation: slow }
    e4@{ animation: fast }
    e5@{ animation: fast }
    e6@{ animation: fast }
    e7@{ animation: fast }
    
    %% Styling
    classDef external fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef github fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef snowflake fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef container fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B external
    class C,D github
    class E,F,G,H snowflake
    class I,J container
    class e3 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **Age Validation** | GDPR compliance enforced - users <14 blocked |
| **Form Processing** | GitHub workflow executes within 30 seconds |
| **Stage Upload** | Data successfully uploaded to Snowflake Stage |
| **Account Creation** | Snowflake user created with correct RBAC role |
| **Email Delivery** | Welcome email sent with credentials |
| **Consent Flow** | Minors (14-17) redirected to consent upload |
| **Error Handling** | Failed workflows queued for manual review |
| **Retry Logic** | Maximum 3 retry attempts with exponential backoff |

## Technical Implementation Notes

### Cost Optimization Pattern
GitHub Actions serves as economic API proxy to minimize external service costs. Snowflake Stages buffer data for daily batch processing instead of real-time table writes.

### RBAC Assignment Logic
- **Participants**: READ access to course materials and certificate generation
- **Volunteers**: WRITE access for content creation and slide generation tools

### Error Recovery Strategy
- Form validation errors return specific messages
- GitHub workflow failures trigger manual review queue
- Snowflake processing errors auto-retry with backoff
