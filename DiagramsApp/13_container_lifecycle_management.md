# Pipeline 13: Container Lifecycle Management

## Overview
Critical infrastructure pipeline that manages daily SPCS container startup, volume mounting, and URL management. Implements T-24h cycle to refresh container state and ensure data synchronization. Manages dynamic URL generation and landing page updates for seamless user access.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `13` |
| **Category** | Infrastructure & Data |
| **Priority** | Very High |
| **Connects To** | `01` (User Registration), `02` (Authentication), `14` (Volume Data Sync), `15` (Landing Page URL Update) |
| **Triggered By** | GitHub Actions daily schedule (T-24h) |
| **Outputs To** | SPCS Container, GitHub Pages, Volume Storage |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% Trigger Layer
    A@{ shape: stadium, label: "GitHub Actions<br/>Daily Scheduler" }
    B@{ shape: cyl, label: "GitHub Secrets<br/>Snowflake Credentials" }
    
    %% Container Management Layer
    C@{ shape: hex, label: "SPCS Container<br/>Lifecycle Controller" }
    D@{ shape: diam, label: "Container State<br/>Health Check" }
    E@{ shape: rounded, label: "Streamlit App<br/>Runtime Environment" }
    
    %% Data Synchronization Layer
    F@{ shape: bow-rect, label: "Snowflake Tables<br/>Source Data" }
    G@{ shape: cyl, label: "Volume Storage<br/>Container Data" }
    H@{ shape: doc, label: "Volume Mount<br/>Data Transfer" }
    
    %% URL Management Layer
    I@{ shape: rounded, label: "Dynamic URL<br/>Generator" }
    J@{ shape: rounded, label: "GitHub Pages<br/>Landing Page" }
    K@{ shape: stadium, label: "Custom Domain<br/>User Access Point" }
    
    %% Critical Flow with Animation
    A e1@--> C
    B -.-> C
    C e2@--> D
    D e3@--> E
    F e4@--> G
    G e5@--> H
    H e6@--> E
    C e7@--> I
    I e8@--> J
    J e9@--> K
    
    %% Animation for Critical Operations
    e1@{ animation: fast }
    e2@{ animation: slow }
    e3@{ animation: fast }
    e4@{ animation: slow }
    e5@{ animation: fast }
    e6@{ animation: fast }
    e7@{ animation: fast }
    e8@{ animation: fast }
    e9@{ animation: fast }
    
    %% Styling
    classDef trigger fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef container fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef data fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef url fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B trigger
    class C,D,E container
    class F,G,H data
    class I,J,K url
    class e2,e4 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **Daily Trigger** | GitHub Actions executes at T-24h schedule |
| **Container Startup** | SPCS container successfully initialized |
| **Volume Mount** | Fresh data loaded from Snowflake tables |
| **Health Check** | Container responds to health endpoint |
| **URL Generation** | New dynamic URL captured and validated |
| **Landing Page Update** | GitHub Pages deployed with new URL |
| **Custom Domain** | DNS routing to updated landing page |
| **Access Validation** | End-to-end user access test passes |

## Technical Implementation Notes

### T-24h Cycle Strategy
Daily container refresh ensures data consistency and security isolation. GitHub Actions scheduler triggers at off-peak hours to minimize user impact during container restart operations.

### Volume Data Synchronization
Snowflake tables export to volume storage using optimized batch queries. Volume mounting occurs during container initialization to ensure fresh data availability for Streamlit applications.

### Dynamic URL Management
Snowflake SPCS generates new URLs on each container restart for security. Landing page automatically updates with latest URL using GitHub Actions deployment pipeline to maintain seamless user experience.

### Error Recovery Strategy
- Container startup failures trigger automatic retry with exponential backoff
- Volume mount errors fall back to previous day's data snapshot
- URL update failures maintain previous working URL until resolution
