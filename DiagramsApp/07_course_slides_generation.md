# Pipeline 07: Course Slides Generation

## Overview
AI-powered slide generation pipeline combining volunteer input forms with Snowflake Cortex AI to create personalized course presentations. Supports multiple output formats (Weasyprint PDF, Impress.js web) with automated web publication for broader accessibility.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `07` |
| **Category** | Content Generation & Document |
| **Priority** | Medium |
| **Connects To** | `20` (Slide Web Publication) |
| **Triggered By** | Course approval (Pipeline 05) + AI content (Pipeline 18) |
| **Outputs To** | PDF slides, Web presentation, Published content |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% Input Layer
    A@{ shape: rounded, label: "Streamlit Form<br/>Volunteer Input" }
    B@{ shape: bow-rect, label: "Course Metadata<br/>Approved Content" }
    
    %% AI Processing Layer
    C@{ shape: hex, label: "Cortex AI Engine<br/>COMPLETE Function" }
    D@{ shape: stadium, label: "Content Generation<br/>Slide Text Creation" }
    E@{ shape: diam, label: "Quality Validation<br/>Content Review" }
    
    %% Template Processing Layer
    F@{ shape: doc, label: "GitHub Templates<br/>Slide Layouts" }
    G@{ shape: rounded, label: "Template Engine<br/>Content Integration" }
    H@{ shape: bow-rect, label: "Container Volumes<br/>Asset Loading" }
    
    %% Multi-Format Rendering Layer
    I@{ shape: hex, label: "Weasyprint Engine<br/>PDF Generation" }
    J@{ shape: hex, label: "Impress.js Engine<br/>Web Presentation" }
    K@{ shape: stadium, label: "Publication Pipeline<br/>Web Deployment" }
    
    %% Dual-Output Generation Flow
    A e1@--> C
    B e2@--> C
    C e3@--> D
    D e4@--> E
    E e5@--> F
    F e6@--> G
    G e7@--> H
    H e8@--> I
    H e9@--> J
    I e10@--> K
    J e11@--> K
    
    %% Animation for AI and Rendering
    e1@{ animation: fast }
    e2@{ animation: fast }
    e3@{ animation: slow }
    e4@{ animation: slow }
    e5@{ animation: fast }
    e6@{ animation: fast }
    e7@{ animation: fast }
    e8@{ animation: slow }
    e9@{ animation: slow }
    e10@{ animation: fast }
    e11@{ animation: fast }
    
    %% Styling
    classDef input fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef ai fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef template fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef render fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B input
    class C,D,E ai
    class F,G,H template
    class I,J,K render
    class e3,e4,e8,e9 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **Form Completion** | All required volunteer input fields populated |
| **Course Data** | Approved course metadata available |
| **AI Generation** | Cortex AI produces coherent slide content |
| **Content Quality** | Generated text meets educational standards |
| **Template Loading** | GitHub templates successfully retrieved |
| **PDF Rendering** | Weasyprint generates valid PDF slides |
| **Web Generation** | Impress.js creates interactive presentation |
| **Web Publication** | Slides deployed to accessible web interface |
| **Volunteer Access** | Creator can download and access presentations |

## Technical Implementation Notes

### AI-Driven Content Creation
Snowflake Cortex AI COMPLETE function processes volunteer inputs and course context to generate educationally appropriate slide content. Maintains consistency with approved course objectives while personalizing delivery style.

### Multi-Format Output Strategy
Dual rendering pipeline supports both traditional PDF downloads for offline use and modern web presentations for interactive delivery. Format selection based on course delivery method and volunteer preference.

### Template Agnostic Design
GitHub-based template system supports multiple slide frameworks. Easy migration from Weasyprint to Impress.js or integration with Gamma.ai for enhanced visual generation capabilities.

### Error Recovery Strategy
- AI generation failures fall back to template-only slides with placeholder content
- PDF rendering errors retry with simplified layouts
- Web generation failures maintain PDF availability while troubleshooting
