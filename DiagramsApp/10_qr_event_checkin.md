# Pipeline 10: QR Event Check-in

## Overview
Real-time event attendance tracking through QR code scanning integrated with Eventbrite registration system. Captures participant attendance data for analytics, triggers post-session workflows, and maintains accurate engagement metrics for program evaluation.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `10` |
| **Category** | Event & Survey Management |
| **Priority** | High |
| **Connects To** | `11` (Post-Session NPS Survey) |
| **Triggered By** | Authenticated participant QR scan |
| **Outputs To** | Attendance record, Event analytics, Survey trigger |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% Scanning Layer
    A@{ shape: rounded, label: "QR Code Scanner<br/>Mobile Interface" }
    B@{ shape: diam, label: "Code Validation<br/>Format & Security" }
    
    %% Event Verification Layer
    C@{ shape: cyl, label: "Eventbrite API<br/>Registration Check" }
    D@{ shape: hex, label: "Event Validation<br/>Time & Location" }
    E@{ shape: stadium, label: "Participant Auth<br/>Identity Verification" }
    
    %% Data Processing Layer
    F@{ shape: bow-rect, label: "GitHub Actions<br/>API Proxy" }
    G@{ shape: cyl, label: "Snowflake Stage<br/>Attendance Buffer" }
    H@{ shape: hex, label: "Data Validation<br/>Duplicate Check" }
    
    %% Recording Layer
    I@{ shape: stadium, label: "Attendance Record<br/>Database Insert" }
    J@{ shape: rounded, label: "Event Analytics<br/>Real-time Metrics" }
    K@{ shape: doc, label: "Survey Trigger<br/>Post-session Flow" }
    
    %% Check-in Process Flow
    A e1@--> B
    B e2@--> C
    C e3@--> D
    D e4@--> E
    E e5@--> F
    F e6@--> G
    G e7@--> H
    H e8@--> I
    I e9@--> J
    I e10@--> K
    
    %% Animation for Real-time Operations
    e1@{ animation: fast }
    e2@{ animation: fast }
    e3@{ animation: slow }
    e4@{ animation: fast }
    e5@{ animation: fast }
    e6@{ animation: fast }
    e7@{ animation: fast }
    e8@{ animation: fast }
    e9@{ animation: fast }
    e10@{ animation: fast }
    
    %% Styling
    classDef scanning fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef verification fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef processing fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef recording fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B scanning
    class C,D,E verification
    class F,G,H processing
    class I,J,K recording
    class e3 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **QR Code Scan** | Valid QR code successfully read |
| **Code Security** | QR format validated against tampering |
| **Event Registration** | Participant registered via Eventbrite |
| **Time Validation** | Check-in within event time window |
| **Location Check** | QR scan at correct event location |
| **Identity Verification** | Participant authentication confirmed |
| **Duplicate Prevention** | Single check-in per participant enforced |
| **Attendance Record** | Database entry created successfully |
| **Analytics Update** | Real-time metrics refreshed |
| **Survey Trigger** | Post-session workflow initiated |

## Technical Implementation Notes

### Real-time Integration
Direct integration with Eventbrite API ensures attendance tracking matches official registration data. GitHub Actions provides cost-effective API proxy while maintaining real-time responsiveness for participant experience.

### Security & Anti-fraud
QR code validation prevents tampering and replay attacks. Time and location constraints ensure attendance authenticity while duplicate detection maintains accurate headcount metrics.

### Cost-Optimized Architecture
GitHub Actions proxy pattern minimizes API costs while Snowflake Stages buffer high-frequency check-ins for batch processing. Real-time analytics balanced with economic data storage strategies.

### Error Recovery Strategy
- Invalid QR codes provide immediate feedback with scan retry guidance
- Eventbrite API failures fall back to manual registration lookup
- Network issues queue check-ins for automatic retry when connectivity restores
