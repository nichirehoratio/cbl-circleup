# Pipeline 11: Post-Session NPS Survey

## Overview
Mandatory NPS survey collection pipeline triggered after event completion. Implements quality gates for certificate unlock and provides critical feedback data for program improvement. Integrates with YouForm or Streamlit interface for participant experience optimization.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `11` |
| **Category** | Event & Survey Management |
| **Priority** | Very High |
| **Connects To** | `06` (Course Certification) |
| **Triggered By** | Event completion and attendance confirmation (Pipeline 10) |
| **Outputs To** | NPS score data, Certificate unlock trigger, Program analytics |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% Trigger Layer
    A@{ shape: stadium, label: "Event Completion<br/>Attendance Signal" }
    B@{ shape: diam, label: "Participant Validation<br/>Eligibility Check" }
    
    %% Survey Interface Layer
    C@{ shape: rounded, label: "Survey Interface<br/>YouForm/Streamlit" }
    D@{ shape: doc, label: "NPS Questions<br/>Standardized Form" }
    E@{ shape: hex, label: "Response Validation<br/>Completeness Check" }
    
    %% Data Processing Layer
    F@{ shape: bow-rect, label: "GitHub Actions<br/>Response Proxy" }
    G@{ shape: cyl, label: "Snowflake Stage<br/>Survey Buffer" }
    H@{ shape: hex, label: "Score Calculation<br/>NPS Analytics" }
    
    %% Quality Gate Layer
    I@{ shape: diam, label: "Quality Threshold<br/>NPS ≥4.2 Check" }
    J@{ shape: stadium, label: "Certificate Unlock<br/>Trigger Signal" }
    K@{ shape: bow-rect, label: "Analytics Database<br/>Program Metrics" }
    
    %% Survey Process Flow
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
    
    %% Animation for Critical Path
    e1@{ animation: fast }
    e2@{ animation: fast }
    e3@{ animation: fast }
    e4@{ animation: fast }
    e5@{ animation: fast }
    e6@{ animation: fast }
    e7@{ animation: fast }
    e8@{ animation: slow }
    e9@{ animation: fast }
    e10@{ animation: fast }
    
    %% Styling
    classDef trigger fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef interface fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef processing fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef quality fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B trigger
    class C,D,E interface
    class F,G,H processing
    class I,J,K quality
    class e8 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **Event Completion** | Attendance confirmed via Pipeline 10 |
| **Participant Eligibility** | Valid participant with complete session |
| **Survey Access** | Interface successfully loaded |
| **Question Delivery** | All NPS questions presented |
| **Response Validation** | Complete survey submission verified |
| **Score Calculation** | NPS score computed accurately |
| **Quality Threshold** | NPS ≥4.2 validation performed |
| **Certificate Trigger** | Pipeline 06 activated on passing score |
| **Analytics Storage** | Survey data stored for program analysis |
| **Participant Notification** | Result communicated to participant |

## Technical Implementation Notes

### Mandatory Survey Completion
Survey completion is prerequisite for certificate generation, ensuring high response rates and quality feedback collection. Progressive prompting and session-based reminders maintain engagement without becoming intrusive.

### Quality-Gated Certificate Unlock
NPS threshold of 4.2 ensures only satisfied participants receive certificates, maintaining program credibility while identifying courses requiring improvement. Failed threshold triggers instructor feedback rather than participant penalty.

### Real-time Analytics Integration
Survey responses immediately contribute to program analytics for rapid course improvement cycles. Aggregate NPS trends inform curriculum development and instructor training priorities.

### Error Recovery Strategy
- Incomplete surveys save progress automatically for later completion
- Network failures queue responses for retry with offline capability
- Low NPS scores trigger constructive feedback collection for course improvement
