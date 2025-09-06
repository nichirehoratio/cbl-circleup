# Circle Up - Technical Diagrams Documentation

## Complete Pipeline Mapping & Dependencies

This document provides a comprehensive overview of all 21 technical pipelines in the Circle Up system, their interconnections, and dependencies. Each pipeline follows the LEGO format for modular development with complete implementation ready.

## Pipeline Status Overview

### ✅ All Pipelines Complete (21/21)
**01-17**: Core operational and advanced pipelines implemented  
**18-21**: AI and media processing pipelines implemented  
🎯 **Ready**: Complete system for modular development deployment

## Pipeline Overview Table

| ID | Pipeline Name | Category | Priority | Triggers | Outputs | Dependencies |
|----|---------------|----------|----------|----------|---------|--------------|
| **01** | User Registration | User Management | Very High | YouForm webhook | Snowflake Users, RBAC, Email | → 02, 04, 13 |
| **02** | User Authentication | User Management | High | User login attempt | Session token, Container access | ← 01 → 13, 03 |
| **03** | Account Deletion | User Management | High | User request | Account cleanup, Data purge | ← 02 |
| **04** | Parental Consent Upload | User Management | Very High | Minor user registration | Compliance record, Access unlock | ← 01 → 13 |
| **05** | Course Proposal Evaluation | User Management | Medium | Volunteer submission | Approval status, Course metadata | ← 02 → 06, 07 |
| **06** | Course Certification | Content Generation | High | NPS survey completion | PDF certificate | ← 11 → 12 |
| **07** | Course Slides Generation | Content Generation | Medium | Form submission + Cortex AI | PDF/Web slides | ← 05, 18 → 20 |
| **08** | Volunteer Legal Documents | Content Generation | Medium | Document request | PDF legal docs | ← 02 → 13 |
| **09** | Social Media Assets | Content Generation | Low | Course metadata | Marketing assets | ← 05 → 13 |
| **10** | QR Event Check-in | Event Management | High | QR scan | Attendance record | ← 02 → 11 |
| **11** | Post-Session NPS Survey | Event Management | Very High | Event completion | NPS score, Certificate unlock | ← 10 → 06 |
| **12** | Meeting Reminders | Communication | Low | Event registration | Automated reminders | External platforms |
| **13** | Container Lifecycle Management | Infrastructure | Very High | Daily T-24h schedule | Container URL, Volume mount | ← 01, 02, 04 → 14, 15 |
| **14** | Volume Data Sync | Infrastructure | High | Container startup | Fresh data volumes | ← 13 |
| **15** | Landing Page URL Update | Infrastructure | High | Container restart | Updated landing page | ← 13 |
| **16** | GitHub Workflow Orchestration | Infrastructure | High | API webhooks | Stage uploads | All pipelines |
| **17** | Audio Processing Deepgram | AI Processing | Medium | Audio upload | Text transcription | ← 02 → 19 |
| **18** | Cortex AI Content Generation | AI Processing | Medium | Form data | AI-generated content | ← 02 → 07 |
| **19** | Podcast Generation Jellypod | AI Processing | Medium | Audio text | Spotify podcast | ← 17 |
| **20** | Slide Web Publication | Integration | Medium | Slide generation | Web-published slides | ← 07 → 15 |
| **21** | Eventbrite Event Creation | Integration | Medium | Course approval | Event listing | ← 05 |

## Critical Dependencies Matrix

### Tier 1 - Foundation (Must Execute First)
- **Pipeline 01** (User Registration) - Enables all user-dependent pipelines
- **Pipeline 13** (Container Lifecycle) - Enables all container-dependent pipelines
- **Pipeline 16** (GitHub Workflow) - Enables all API integrations

### Tier 2 - Core Operations (Dependent on Tier 1)
- **Pipeline 02** (Authentication) ← 01
- **Pipeline 14** (Volume Sync) ← 13
- **Pipeline 15** (Landing Page) ← 13

### Tier 3 - Business Logic (Dependent on Tier 2)
- **Pipeline 04** (Parental Consent) ← 01, 13
- **Pipeline 05** (Course Proposal) ← 02
- **Pipeline 10** (Event Check-in) ← 02

### Tier 4 - Content & AI (Dependent on Tier 3)
- **Pipeline 07** (Slides Generation) ← 05, 18
- **Pipeline 17** (Audio Processing) ← 02
- **Pipeline 18** (Cortex AI) ← 02

### Tier 5 - Outputs & Publications (Dependent on Tier 4)
- **Pipeline 06** (Certification) ← 11
- **Pipeline 19** (Podcast) ← 17
- **Pipeline 20** (Web Publication) ← 07

## Integration Patterns

### Cost-Optimized Data Flow
```
External API → GitHub Actions (Proxy) → Snowflake Stage (Buffer) → Daily Tasks → Tables
```
**Used by**: Pipelines 01, 05, 10, 11, 16, 21

### Container Data Access
```
Snowflake Tables → Volume Export → Volume Mount → Container Access
```
**Used by**: Pipelines 13, 14, 04, 07, 08, 17, 18

### AI Content Generation
```
Form Input → Cortex AI Processing → Content Output → Web Publication
```
**Used by**: Pipelines 07, 18, 17, 19, 20

### Dynamic URL Management
```
Container Restart → New URL → Landing Page Update → GitHub Pages Deploy
```
**Used by**: Pipelines 13, 15, 20

## Error Handling Hierarchy

| Level | Scope | Recovery Method | Affected Pipelines |
|-------|-------|-----------------|-------------------|
| **Critical** | System-wide failure | Manual intervention + rollback | 01, 13, 16 |
| **High** | Service disruption | Automatic retry + fallback | 02, 14, 15 |
| **Medium** | Feature degradation | Retry + graceful degradation | 05, 10, 11 |
| **Low** | Individual operation | Simple retry | 06, 07, 08, 09 |

## Development Sequence Recommendations

### Phase 1 - Infrastructure
1. Pipeline 16 (GitHub Workflow)
2. Pipeline 13 (Container Lifecycle)
3. Pipeline 14 (Volume Sync)
4. Pipeline 15 (Landing Page)

### Phase 2 - User Foundation
5. Pipeline 01 (User Registration)
6. Pipeline 02 (User Authentication)
7. Pipeline 04 (Parental Consent)
8. Pipeline 03 (Account Deletion)

### Phase 3 - Core Business
9. Pipeline 05 (Course Proposal)
10. Pipeline 10 (Event Check-in)
11. Pipeline 11 (NPS Survey)
12. Pipeline 06 (Certification)

### Phase 4 - AI & Content
13. Pipeline 18 (Cortex AI)
14. Pipeline 07 (Slides Generation)
15. Pipeline 17 (Audio Processing)
16. Pipeline 19 (Podcast Generation)

### Phase 5 - Integrations
17. Pipeline 20 (Web Publication)
18. Pipeline 21 (Eventbrite Integration)
19. Pipeline 08 (Legal Documents)
20. Pipeline 09 (Social Media)
21. Pipeline 12 (Meeting Reminders)