# Pipeline 14: Volume Data Sync

## Overview
Data synchronization pipeline that exports fresh data from Snowflake tables to container volumes during SPCS startup. Ensures containers have current participant, course, and operational data while maintaining cost-effective batch processing strategy.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `14` |
| **Category** | Infrastructure & Data |
| **Priority** | High |
| **Connects To** | None (Terminal data pipeline) |
| **Triggered By** | Container startup (Pipeline 13) |
| **Outputs To** | Fresh data volumes, Container data access, Application state |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% Source Data Layer
    A@{ shape: cyl, label: "Snowflake Tables<br/>Production Data" }
    B@{ shape: bow-rect, label: "User Registry<br/>Account Information" }
    C@{ shape: cyl, label: "Course Database<br/>Content Metadata" }
    
    %% Export Processing Layer
    D@{ shape: hex, label: "Data Export Engine<br/>Batch Query Processor" }
    E@{ shape: stadium, label: "Format Conversion<br/>Container-Compatible" }
    F@{ shape: rounded, label: "Data Validation<br/>Integrity Check" }
    
    %% Volume Management Layer
    G@{ shape: bow-rect, label: "Volume Storage<br/>Container File System" }
    H@{ shape: hex, label: "Volume Mount<br/>Data Attachment" }
    I@{ shape: stadium, label: "Access Permissions<br/>Security Configuration" }
    
    %% Container Integration Layer
    J@{ shape: rounded, label: "SPCS Container<br/>Application Runtime" }
    K@{ shape: doc, label: "Data Availability<br/>Application Access" }
    L@{ shape: cyl, label: "Sync Verification<br/>Completeness Check" }
    
    %% Data Sync Flow
    A e1@--> D
    B e2@--> D
    C e3@--> D
    D e4@--> E
    E e5@--> F
    F e6@--> G
    G e7@--> H
    H e8@--> I
    I e9@--> J
    J e10@--> K
    K e11@--> L
    
    %% Animation for Data Operations
    e1@{ animation: fast }
    e2@{ animation: fast }
    e3@{ animation: fast }
    e4@{ animation: slow }
    e5@{ animation: fast }
    e6@{ animation: fast }
    e7@{ animation: slow }
    e8@{ animation: fast }
    e9@{ animation: fast }
    e10@{ animation: fast }
    e11@{ animation: fast }
    
    %% Styling
    classDef source fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef export fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef volume fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef container fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B,C source
    class D,E,F export
    class G,H,I volume
    class J,K,L container
    class e4,e7 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **Data Export** | All required tables successfully queried |
| **Format Conversion** | Data converted to container-compatible formats |
| **Integrity Validation** | Data consistency and completeness verified |
| **Volume Creation** | Container volumes successfully provisioned |
| **Mount Operation** | Volumes attached to container runtime |
| **Permission Setup** | Container access permissions configured |
| **Application Access** | Streamlit apps can read mounted data |
| **Sync Verification** | All expected data files present and accessible |
| **Performance Check** | Data loading completes within time limits |

## Technical Implementation Notes

### Batch Export Strategy
Daily batch processing minimizes Snowflake compute costs while ensuring containers have sufficiently fresh data. Export timing optimized for off-peak hours to reduce resource contention and operational costs.

### Container Isolation Pattern
Volume-based data access ensures containers cannot directly query warehouse, maintaining security boundaries while providing necessary data access. Eliminates need for database credentials in container environment.

### Data Freshness Balance
T-24h refresh cycle balances data currency requirements with cost optimization. Critical real-time data handled through separate pipelines while bulk operational data refreshed daily.

### Error Recovery Strategy
- Export failures retry with incremental data sets to isolate problematic records
- Volume mount errors fall back to previous day's data with staleness warnings
- Container access failures trigger automatic remount with permission reset
