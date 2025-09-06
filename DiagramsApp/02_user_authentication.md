# Pipeline 02: User Authentication

## Overview
Core authentication pipeline that validates user credentials against Snowflake's native authentication system. Implements session management with RBAC validation and container access provisioning. Handles password policies and security controls for both participants and volunteers.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `02` |
| **Category** | User Management & Data Capture |
| **Priority** | High |
| **Connects To** | `13` (Container Lifecycle), `03` (Account Deletion), `05` (Course Proposal), `10` (Event Check-in) |
| **Triggered By** | User login attempt |
| **Outputs To** | Session token, Container access, RBAC validation |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% User Input Layer
    A@{ shape: rounded, label: "Login Interface<br/>User Credentials" }
    B@{ shape: doc, label: "Password Policy<br/>Validation Rules" }
    
    %% Authentication Core Layer
    C@{ shape: cyl, label: "Snowflake Auth<br/>Native Authentication" }
    D@{ shape: diam, label: "Credential Validation<br/>Security Check" }
    E@{ shape: hex, label: "RBAC Engine<br/>Permission Resolution" }
    
    %% Session Management Layer
    F@{ shape: stadium, label: "Session Token<br/>Generation" }
    G@{ shape: bow-rect, label: "Session Storage<br/>Token Management" }
    H@{ shape: rounded, label: "Session Expiry<br/>24h TTL" }
    
    %% Container Access Layer
    I@{ shape: rounded, label: "SPCS Container<br/>Access Gateway" }
    J@{ shape: doc, label: "Container Permissions<br/>Role-based Access" }
    K@{ shape: stadium, label: "Streamlit App<br/>User Interface" }
    
    %% Critical Authentication Flow
    A e1@--> D
    B -.-> D
    D e2@--> C
    C e3@--> E
    E e4@--> F
    F e5@--> G
    G e6@--> H
    E e7@--> J
    J e8@--> I
    I e9@--> K
    
    %% Animation for Security Operations
    e1@{ animation: fast }
    e2@{ animation: slow }
    e3@{ animation: fast }
    e4@{ animation: fast }
    e5@{ animation: fast }
    e6@{ animation: fast }
    e7@{ animation: fast }
    e8@{ animation: fast }
    e9@{ animation: fast }
    
    %% Styling
    classDef input fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef auth fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef session fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef container fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B input
    class C,D,E auth
    class F,G,H session
    class I,J,K container
    class e2 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **Credential Validation** | Snowflake native auth validates user/password |
| **Password Policy** | Complex password rules enforced |
| **RBAC Resolution** | Correct role permissions assigned |
| **Session Generation** | Valid JWT token created with expiry |
| **Container Access** | User can access SPCS Streamlit interface |
| **Session Expiry** | 24-hour TTL enforced automatically |
| **Role Separation** | Participant vs Volunteer access differentiated |
| **Security Logging** | Authentication attempts logged for audit |

## Technical Implementation Notes

### Snowflake Native Authentication
Leverages Snowflake's built-in user management and authentication system. No external identity providers required, reducing complexity and attack surface while maintaining enterprise-grade security controls.

### RBAC Permission Model
- **Participants**: READ access to course materials, certificate downloads, survey completion
- **Volunteers**: WRITE access for course creation, slide generation, legal document access, content management tools

### Session Security Strategy
JWT tokens with 24-hour expiration stored in secure session storage. Automatic renewal on active usage with sliding window expiration to balance security and user experience.

### Error Recovery Strategy
- Invalid credentials return specific error messages without revealing user existence
- Account lockout after multiple failed attempts with exponential backoff
- Session corruption triggers automatic re-authentication flow
