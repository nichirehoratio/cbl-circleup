# Pipeline 09: Social Media Assets

## Overview
Marketing asset generation pipeline that transforms approved course metadata into promotional materials. Utilizes course information to create PDF marketing documents, converts to images via PDF2Image, and provides download interface for volunteer-driven social media promotion.

## LEGO Reference Table

| **Field** | **Value** |
|-----------|-----------|
| **Pipeline ID** | `09` |
| **Category** | Content Generation & Document |
| **Priority** | Low |
| **Connects To** | `13` (Container Lifecycle) |
| **Triggered By** | Course approval and metadata availability (Pipeline 05) |
| **Outputs To** | Marketing PDFs, Social media images, Asset downloads |

## Stack Architecture

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor':'#f0f8ff','primaryTextColor':'#2c3e50','primaryBorderColor':'#3498db','lineColor':'#2980b9','secondaryColor':'#e8f4fd','tertiaryColor':'#d5e8f3','background':'#ffffff','mainBkg':'#f0f8ff','secondBkg':'#e1f0ff','tertiaryBkg':'#d1e7ff'}}}%%
flowchart TD
    %% Source Data Layer
    A@{ shape: bow-rect, label: "Course Metadata<br/>Approved Content" }
    B@{ shape: cyl, label: "Volunteer Information<br/>Instructor Details" }
    
    %% Asset Design Layer
    C@{ shape: doc, label: "GitHub Templates<br/>Marketing Layouts" }
    D@{ shape: hex, label: "Brand Guidelines<br/>Visual Standards" }
    E@{ shape: stadium, label: "Content Adaptation<br/>Marketing Copy" }
    
    %% Generation Pipeline Layer
    F@{ shape: rounded, label: "Template Engine<br/>Asset Composition" }
    G@{ shape: hex, label: "Weasyprint Engine<br/>PDF Generation" }
    H@{ shape: stadium, label: "PDF2Image Converter<br/>Multi-format Output" }
    
    %% Distribution Layer
    I@{ shape: doc, label: "Asset Gallery<br/>Generated Materials" }
    J@{ shape: rounded, label: "Download Interface<br/>Volunteer Access" }
    K@{ shape: bow-rect, label: "Usage Analytics<br/>Download Tracking" }
    
    %% Marketing Asset Flow
    A e1@--> E
    B e2@--> E
    C e3@--> F
    D e4@--> F
    E e5@--> F
    F e6@--> G
    G e7@--> H
    H e8@--> I
    I e9@--> J
    J e10@--> K
    
    %% Animation for Generation Process
    e1@{ animation: fast }
    e2@{ animation: fast }
    e3@{ animation: fast }
    e4@{ animation: fast }
    e5@{ animation: fast }
    e6@{ animation: slow }
    e7@{ animation: slow }
    e8@{ animation: slow }
    e9@{ animation: fast }
    e10@{ animation: fast }
    
    %% Styling
    classDef source fill:#e8f4fd,stroke:#3498db,stroke-width:2px
    classDef design fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    classDef generation fill:#dda0dd,stroke:#9932cc,stroke-width:2px
    classDef distribution fill:#a8e6cf,stroke:#00b894,stroke-width:2px
    classDef critical stroke-dasharray: 5,5,stroke-dashoffset: 10,animation: dash 2s linear infinite
    
    class A,B source
    class C,D,E design
    class F,G,H generation
    class I,J,K distribution
    class e6,e7,e8 critical
```

## Definition of Done (DoD)

| **Criteria** | **Validation Method** |
|--------------|----------------------|
| **Course Data** | Approved course metadata available |
| **Instructor Info** | Volunteer details for attribution |
| **Template Loading** | Marketing templates retrieved from GitHub |
| **Brand Compliance** | Visual guidelines applied correctly |
| **Content Adaptation** | Course details formatted for marketing |
| **PDF Generation** | Weasyprint creates marketing documents |
| **Image Conversion** | PDF2Image produces social media formats |
| **Asset Gallery** | Generated materials available for review |
| **Download Access** | Volunteers can access marketing assets |
| **Usage Tracking** | Download analytics recorded |

## Technical Implementation Notes

### Automated Marketing Generation
Transforms technical course metadata into compelling marketing copy suitable for social media promotion. Maintains consistent brand voice while highlighting unique course value propositions.

### Multi-Format Asset Creation
Dual-output pipeline generates both high-quality PDFs for print materials and optimized images for digital social media platforms. Supports various aspect ratios and resolution requirements.

### Brand Consistency Framework
GitHub-based template management ensures all marketing materials maintain visual brand standards. Automated application of logos, colors, fonts, and layout guidelines across all generated assets.

### Error Recovery Strategy
- Missing course data triggers notification with template-only asset generation
- PDF generation failures retry with simplified layouts
- Image conversion errors fall back to PDF-only output with manual conversion guidance
