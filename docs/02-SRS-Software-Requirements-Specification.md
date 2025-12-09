# Software Requirements Specification (SRS)
## AI-Driven Mental Health Early Detection System
### Project Codename: "MindGuard"

---

## Document Control

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2024-12-09 | Development Team | Initial SRS |

---

## 1. Introduction

### 1.1 Purpose
This Software Requirements Specification (SRS) document provides a comprehensive description of the MindGuard system's functional and non-functional requirements. It serves as the primary reference for the development team and stakeholders.

### 1.2 Scope
MindGuard is a multi-modal AI-powered mental health early detection platform that analyzes text, voice, and behavioral data to generate personalized risk assessments and actionable recommendations.

### 1.3 Definitions and Acronyms

| Term | Definition |
|------|------------|
| **API** | Application Programming Interface |
| **BERT** | Bidirectional Encoder Representations from Transformers |
| **EHR/EMR** | Electronic Health/Medical Records |
| **GDPR** | General Data Protection Regulation |
| **HIPAA** | Health Insurance Portability and Accountability Act |
| **NLP** | Natural Language Processing |
| **PHI** | Protected Health Information |
| **REST** | Representational State Transfer |
| **SDK** | Software Development Kit |

### 1.4 References
- BRD-MindGuard-v1.0
- HIPAA Security Rule (45 CFR Part 160 and Subparts A and C of Part 164)
- GDPR Regulation (EU) 2016/679
- IEEE Std 830-1998 (SRS Standard)

---

## 2. Overall Description

### 2.1 Product Perspective

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                           MINDGUARD SYSTEM CONTEXT                                 │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                    │
│   ┌──────────────┐                                      ┌──────────────┐          │
│   │   Mobile     │◄────────────────────────────────────►│   Backend    │          │
│   │   App        │         REST API / WebSocket         │   Services   │          │
│   │   (iOS/And)  │                                      │              │          │
│   └──────────────┘                                      └──────┬───────┘          │
│          │                                                      │                  │
│          │                                                      │                  │
│   ┌──────▼──────┐                                      ┌───────▼───────┐          │
│   │  Edge AI    │                                      │   Cloud AI    │          │
│   │  Processing │                                      │   Services    │          │
│   │  (On-Device)│                                      │   (ML Models) │          │
│   └─────────────┘                                      └───────┬───────┘          │
│                                                                 │                  │
│                                                        ┌───────▼───────┐          │
│                                                        │   External    │          │
│   ┌─────────────┐      ┌─────────────┐                │   Services    │          │
│   │  Wearables  │      │   Third     │                │ ─────────────  │          │
│   │  (Health    │      │   Party     │                │ • Apple Health │          │
│   │   APIs)     │      │   Systems   │                │ • Google Fit   │          │
│   └─────────────┘      │   (EHR)     │                │ • Push Notif.  │          │
│                        └─────────────┘                └───────────────┘          │
│                                                                                    │
└──────────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Product Functions Summary

| Module | Primary Functions |
|--------|-------------------|
| **User Mobile App** | Self-assessment, journaling, data collection, risk viewing, recommendations |
| **NLP Engine** | Text sentiment analysis, cognitive distortion detection, vocabulary complexity tracking |
| **Voice Analysis Engine** | Prosodic feature extraction, emotion recognition from voice patterns |
| **Behavioral Analysis Engine** | Sleep/activity pattern analysis, anomaly detection |
| **Risk Scoring Engine** | Multi-modal fusion, personalized risk calculation |
| **Recommendation Engine** | Context-aware intervention suggestions |
| **Admin Dashboard** | Aggregate analytics, system monitoring, compliance reporting |
| **Clinical Portal** | Triage interface, patient risk summaries |

### 2.3 User Classes and Characteristics

| User Class | Description | Technical Skill | Usage Frequency |
|------------|-------------|-----------------|-----------------|
| **End User** | Employee/Student using the app | Basic-Intermediate | Daily |
| **Wellness Admin** | HR/Wellness team member | Intermediate | Weekly |
| **Clinician** | Licensed mental health professional | Basic-Intermediate | Daily |
| **System Admin** | IT professional managing the platform | Advanced | As needed |
| **Data Scientist** | ML engineer maintaining models | Expert | Weekly |

### 2.4 Operating Environment

| Component | Requirement |
|-----------|-------------|
| **Mobile App (iOS)** | iOS 14.0 or later |
| **Mobile App (Android)** | Android 10 (API 29) or later |
| **Web Dashboard** | Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ |
| **Backend Services** | Kubernetes on Azure AKS / GCP GKE |
| **Database** | PostgreSQL 14+, MongoDB 5.0+ |
| **AI/ML Platform** | Azure ML / GCP Vertex AI |

### 2.5 Design and Implementation Constraints

| Constraint | Rationale |
|------------|-----------|
| Edge processing for voice data | Privacy - semantic content never transmitted |
| Zero-knowledge encryption for sensitive data | HIPAA/GDPR compliance |
| Maximum 500ms response time for API calls | User experience |
| Offline capability for core features | Accessibility |
| Multi-region data residency | GDPR data sovereignty |

---

## 3. System Features

### 3.1 Feature: User Registration and Consent Management

#### 3.1.1 Description
Secure user registration with explicit consent management for data collection.

#### 3.1.2 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-REG-001 | System shall allow users to register using email, phone, or SSO | Must |
| FR-REG-002 | System shall present consent forms with clear data usage explanations | Must |
| FR-REG-003 | System shall require explicit opt-in for each data stream (text, voice, behavioral) | Must |
| FR-REG-004 | System shall allow users to view and modify consent settings at any time | Must |
| FR-REG-005 | System shall log all consent changes with timestamps | Must |
| FR-REG-006 | System shall support anonymous mode (no identifiable data stored) | Should |
| FR-REG-007 | System shall enforce MFA for sensitive account actions | Must |

---

### 3.2 Feature: Text Analysis (NLP Engine)

#### 3.2.1 Description
Natural Language Processing engine for analyzing text data from journaling, chat, and other sources.

#### 3.2.2 Data Flow

```
┌───────────────┐     ┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│   Text Input  │     │   Pre-       │     │   NLP Model   │     │   Feature     │
│   Sources     │────►│   Processing │────►│   Pipeline    │────►│   Extraction  │
│               │     │              │     │               │     │               │
│ • Journal     │     │ • Tokenize   │     │ • BERT/RoBERTa│     │ • Sentiment   │
│ • Chat        │     │ • Clean      │     │ • Fine-tuned  │     │ • Distortions │
│ • Tickets     │     │ • Normalize  │     │ • Mental H.   │     │ • Complexity  │
└───────────────┘     └───────────────┘     └───────────────┘     └───────────────┘
```

#### 3.2.3 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-NLP-001 | System shall analyze sentiment polarity (positive/negative/neutral) with 85%+ accuracy | Must |
| FR-NLP-002 | System shall detect cognitive distortions (absolutist language: "always", "never", "totally") | Must |
| FR-NLP-003 | System shall track vocabulary complexity trends over time | Must |
| FR-NLP-004 | System shall identify depression/anxiety linguistic markers | Must |
| FR-NLP-005 | System shall process text in English (MVP), with extensibility for other languages | Must |
| FR-NLP-006 | System shall support real-time analysis (< 2 seconds per text block) | Must |
| FR-NLP-007 | System shall detect context and irony to reduce false positives | Should |
| FR-NLP-008 | System shall NOT store raw text after feature extraction | Must |

#### 3.2.4 Cognitive Distortion Detection

| Distortion Type | Example Markers | Clinical Relevance |
|-----------------|-----------------|-------------------|
| **All-or-Nothing Thinking** | "always", "never", "completely", "totally" | High - Depression |
| **Catastrophizing** | "disaster", "terrible", "worst", "ruined" | High - Anxiety |
| **Mind Reading** | "they think", "everyone knows", "they hate" | Medium - Anxiety |
| **Overgeneralization** | "everyone", "no one", "always" | High - Depression |
| **Should Statements** | "should", "must", "ought to", "have to" | Medium - Perfectionism |

---

### 3.3 Feature: Voice Prosody Analysis

#### 3.3.1 Description
Voice analysis engine that extracts emotional indicators from speech patterns WITHOUT analyzing semantic content.

#### 3.3.2 Processing Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      VOICE PROSODY ANALYSIS PIPELINE                              │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                   │
│   ┌──────────────────┐        ┌──────────────────┐        ┌──────────────────┐   │
│   │  EDGE DEVICE     │        │  FEATURE         │        │  CLOUD           │   │
│   │  PROCESSING      │        │  TRANSMISSION    │        │  ANALYSIS        │   │
│   │                  │        │                  │        │                  │   │
│   │  Raw Audio       │        │  Numerical       │        │  Pattern         │   │
│   │       │          │        │  Features Only   │        │  Recognition     │   │
│   │       ▼          │        │       │          │        │       │          │   │
│   │  ┌──────────┐    │   ══►  │       │          │   ══►  │       ▼          │   │
│   │  │ Feature  │    │  (No   │       ▼          │        │  ┌──────────┐    │   │
│   │  │ Extract  │    │  Words)│  ┌──────────┐    │        │  │ Emotion  │    │   │
│   │  └────┬─────┘    │        │  │ Encrypted│    │        │  │ Classif. │    │   │
│   │       │          │        │  │ Vector   │    │        │  └──────────┘    │   │
│   │       ▼          │        │  └──────────┘    │        │                  │   │
│   │  Discard Audio   │        │                  │        │  Risk Score      │   │
│   │                  │        │                  │        │  Contribution    │   │
│   └──────────────────┘        └──────────────────┘        └──────────────────┘   │
│                                                                                   │
│   🔒 PRIVACY: Raw audio NEVER leaves device. Only numeric features transmitted.  │
│                                                                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

#### 3.3.3 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-VOC-001 | System shall extract acoustic features on-device (edge processing) | Must |
| FR-VOC-002 | System shall analyze jitter (frequency instability) | Must |
| FR-VOC-003 | System shall analyze shimmer (amplitude instability) | Must |
| FR-VOC-004 | System shall analyze speaking rate variations | Must |
| FR-VOC-005 | System shall detect hesitation markers and pauses | Must |
| FR-VOC-006 | System shall classify emotional state (neutral/stressed/depressed/anxious) | Must |
| FR-VOC-007 | System shall NOT transmit or store semantic content | Must |
| FR-VOC-008 | System shall process audio within 5 seconds of recording completion | Should |
| FR-VOC-009 | System shall work with ambient noise levels up to 60dB | Should |

#### 3.3.4 Prosodic Feature Specifications

| Feature | Description | Depression Indicator | Anxiety Indicator |
|---------|-------------|---------------------|------------------|
| **Jitter** | Frequency variation between voice cycles | Elevated | Normal |
| **Shimmer** | Amplitude variation between voice cycles | Elevated | Elevated |
| **F0 (Pitch)** | Fundamental frequency | Reduced range | Elevated |
| **Speaking Rate** | Words per minute proxy | Decreased | Increased |
| **Pause Duration** | Time between utterances | Increased | Decreased |
| **Voice Breaks** | Interruptions in phonation | Increased | Elevated |

---

### 3.4 Feature: Behavioral Anomaly Detection

#### 3.4.1 Description
Analysis of behavioral patterns from wearables and device usage to detect deviations from personal baselines.

#### 3.4.2 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-BHV-001 | System shall integrate with Apple HealthKit API | Must |
| FR-BHV-002 | System shall integrate with Google Fit API | Must |
| FR-BHV-003 | System shall establish personal baseline from 14+ days of data | Must |
| FR-BHV-004 | System shall detect sleep pattern anomalies (duration, timing, quality) | Must |
| FR-BHV-005 | System shall detect physical activity anomalies | Must |
| FR-BHV-006 | System shall track screen time and app usage patterns | Should |
| FR-BHV-007 | System shall detect social interaction pattern changes | Should |
| FR-BHV-008 | System shall flag multi-day anomaly clusters | Must |
| FR-BHV-009 | System shall adjust for weekday/weekend patterns | Must |
| FR-BHV-010 | System shall integrate with corporate email APIs (optional, with consent) | Could |

#### 3.4.3 Behavioral Indicators

| Behavior | Normal Variability | Warning Threshold | Critical Threshold |
|----------|-------------------|-------------------|-------------------|
| **Sleep Duration** | ±1.5 hours | ±3 hours for 3+ days | ±4 hours for 5+ days |
| **Step Count** | ±30% | -50% for 3+ days | -70% for 5+ days |
| **Screen Time** | ±20% | +50% for 3+ days | +80% for 5+ days |
| **Social App Usage** | ±25% | -60% for 5+ days | -80% for 7+ days |

---

### 3.5 Feature: Risk Scoring Engine

#### 3.5.1 Description
Multi-modal fusion engine that combines signals from all data streams to generate personalized risk scores.

#### 3.5.2 Risk Score Architecture

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                          RISK SCORING ARCHITECTURE                                │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                    │
│   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐                             │
│   │   NLP       │   │   Voice     │   │  Behavioral │                             │
│   │   Features  │   │   Features  │   │  Features   │                             │
│   │   (0-100)   │   │   (0-100)   │   │  (0-100)    │                             │
│   └──────┬──────┘   └──────┬──────┘   └──────┬──────┘                             │
│          │                 │                 │                                     │
│          │    Weight: 0.4  │    Weight: 0.3  │    Weight: 0.3                     │
│          │                 │                 │                                     │
│          └─────────────────┼─────────────────┘                                     │
│                            │                                                       │
│                            ▼                                                       │
│                   ┌─────────────────┐                                              │
│                   │  FUSION MODEL   │                                              │
│                   │                 │                                              │
│                   │  Weighted +     │                                              │
│                   │  Temporal +     │                                              │
│                   │  Personal       │                                              │
│                   │  Calibration    │                                              │
│                   └────────┬────────┘                                              │
│                            │                                                       │
│                            ▼                                                       │
│                   ┌─────────────────┐        ┌─────────────────────┐              │
│                   │  RISK SCORE     │        │   RISK LEVELS       │              │
│                   │  0 - 100        │───────►│                     │              │
│                   │                 │        │   0-25:  🟢 Low     │              │
│                   │  + Trend        │        │  26-50:  🟡 Moderate │              │
│                   │  + Confidence   │        │  51-75:  🟠 Elevated │              │
│                   │                 │        │  76-100: 🔴 High    │              │
│                   └─────────────────┘        └─────────────────────┘              │
│                                                                                    │
└──────────────────────────────────────────────────────────────────────────────────┘
```

#### 3.5.3 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-RSK-001 | System shall calculate composite risk score (0-100) | Must |
| FR-RSK-002 | System shall provide risk level classification (Low/Moderate/Elevated/High) | Must |
| FR-RSK-003 | System shall calculate risk trend (improving/stable/declining) | Must |
| FR-RSK-004 | System shall provide confidence level for risk assessment | Must |
| FR-RSK-005 | System shall update risk score within 24 hours of new data | Must |
| FR-RSK-006 | System shall weight data streams based on data quality/availability | Must |
| FR-RSK-007 | System shall flag sudden risk score changes (>20 points in 48 hours) | Must |
| FR-RSK-008 | System shall support personalized baseline calibration | Should |
| FR-RSK-009 | System shall generate risk breakdown by category (depression/anxiety/burnout) | Should |

---

### 3.6 Feature: Recommendation Engine

#### 3.6.1 Description
Context-aware recommendation system that suggests appropriate interventions based on risk level and user preferences.

#### 3.6.2 Recommendation Matrix

| Risk Level | Self-Help | Coaching | Clinical |
|------------|-----------|----------|----------|
| **🟢 Low (0-25)** | Wellness tips, maintenance exercises | Optional | Not recommended |
| **🟡 Moderate (26-50)** | Guided exercises, journaling prompts | Suggested | Optional |
| **🟠 Elevated (51-75)** | Intensive self-help modules | Recommended | Available |
| **🔴 High (76-100)** | Crisis resources | Required | Strongly recommended |

#### 3.6.3 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-REC-001 | System shall recommend self-help resources based on risk level | Must |
| FR-REC-002 | System shall suggest breathing/mindfulness exercises | Must |
| FR-REC-003 | System shall recommend coaching sessions when appropriate | Must |
| FR-REC-004 | System shall recommend clinical referral for high-risk users | Must |
| FR-REC-005 | System shall display crisis hotline for critical situations | Must |
| FR-REC-006 | System shall track recommendation engagement and adjust | Should |
| FR-REC-007 | System shall personalize recommendations based on preferences | Should |
| FR-REC-008 | System shall schedule check-in reminders | Should |

---

### 3.7 Feature: User Mobile Application

#### 3.7.1 Description
Native mobile application for iOS and Android providing the end-user interface.

#### 3.7.2 Screen Requirements

| Screen | Key Functions | Requirements |
|--------|---------------|--------------|
| **Onboarding** | Registration, consent, initial assessment | FR-APP-001 to 005 |
| **Home Dashboard** | Risk overview, quick actions, streak tracking | FR-APP-006 to 010 |
| **Journal** | Free-form journaling, guided prompts | FR-APP-011 to 015 |
| **Voice Check-in** | Record voice sample for analysis | FR-APP-016 to 020 |
| **Insights** | Risk history, trends, breakdowns | FR-APP-021 to 025 |
| **Resources** | Self-help content, exercises, videos | FR-APP-026 to 030 |
| **Settings** | Privacy controls, notifications, data export | FR-APP-031 to 035 |

#### 3.7.3 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-APP-001 | App shall support biometric authentication (Face ID, fingerprint) | Must |
| FR-APP-002 | App shall work offline for journaling and self-assessment | Must |
| FR-APP-003 | App shall sync data when connectivity is restored | Must |
| FR-APP-004 | App shall display risk score with visual indicators | Must |
| FR-APP-005 | App shall provide push notifications for check-in reminders | Must |
| FR-APP-006 | App shall allow export of personal data (GDPR) | Must |
| FR-APP-007 | App shall support complete account deletion | Must |
| FR-APP-008 | App shall display privacy status and active consents | Must |
| FR-APP-009 | App shall provide emergency crisis resources accessible without login | Must |
| FR-APP-010 | App shall support accessibility features (VoiceOver, TalkBack) | Must |

---

### 3.8 Feature: Admin Dashboard

#### 3.8.1 Description
Web-based dashboard for organizational administrators to view aggregate insights.

#### 3.8.2 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-ADM-001 | Dashboard shall display aggregate well-being scores (anonymized) | Must |
| FR-ADM-002 | Dashboard shall show adoption metrics (enrollment, active users) | Must |
| FR-ADM-003 | Dashboard shall display trend analytics over time | Must |
| FR-ADM-004 | Dashboard shall support filtering by department/team (if k>20) | Should |
| FR-ADM-005 | Dashboard shall generate compliance reports | Must |
| FR-ADM-006 | Dashboard shall never display individual-level data | Must |
| FR-ADM-007 | Dashboard shall export reports in PDF/CSV formats | Should |
| FR-ADM-008 | Dashboard shall support RBAC (Role-Based Access Control) | Must |

---

### 3.9 Feature: Clinical Portal

#### 3.9.1 Description
Secure web portal for licensed mental health professionals to access triage information.

#### 3.9.2 Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-CLI-001 | Portal shall display risk-ranked patient queue | Must |
| FR-CLI-002 | Portal shall show patient risk summaries (with consent) | Must |
| FR-CLI-003 | Portal shall display risk trend graphs | Must |
| FR-CLI-004 | Portal shall allow clinicians to add notes | Should |
| FR-CLI-005 | Portal shall support secure messaging with patients | Should |
| FR-CLI-006 | Portal shall integrate with EHR systems (Phase 3) | Could |
| FR-CLI-007 | Portal shall log all access for audit purposes | Must |

---

## 4. External Interface Requirements

### 4.1 User Interfaces

| Interface | Type | Description |
|-----------|------|-------------|
| Mobile App | Native iOS/Android | Primary end-user interface |
| Admin Dashboard | Web (React) | Organizational management |
| Clinical Portal | Web (React) | Healthcare provider interface |

### 4.2 Hardware Interfaces

| Interface | Description |
|-----------|-------------|
| Device Microphone | Voice sample capture for prosody analysis |
| Wearable Sensors | Heart rate, step count, sleep data via APIs |
| Biometric Sensors | Face ID / Fingerprint for authentication |

### 4.3 Software Interfaces

| Interface | Type | Purpose |
|-----------|------|---------|
| Apple HealthKit | REST API | Health and activity data |
| Google Fit | REST API | Health and activity data |
| Firebase Cloud Messaging | Push API | Notifications |
| Azure AD / Okta | OAuth 2.0 | SSO Authentication |
| Twilio | REST API | SMS notifications |
| Stripe | REST API | Subscription management |

### 4.4 Communication Interfaces

| Protocol | Usage |
|----------|-------|
| HTTPS/TLS 1.3 | All API communications |
| WebSocket (WSS) | Real-time updates |
| gRPC | Internal microservice communication |

---

## 5. Non-Functional Requirements

### 5.1 Performance Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-PRF-001 | API response time (95th percentile) | < 500ms |
| NFR-PRF-002 | Text analysis processing time | < 2 seconds |
| NFR-PRF-003 | Voice feature extraction (on-device) | < 5 seconds |
| NFR-PRF-004 | Risk score update frequency | Within 24 hours |
| NFR-PRF-005 | App launch time (cold start) | < 3 seconds |
| NFR-PRF-006 | Concurrent users supported | 50,000+ |

### 5.2 Security Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| NFR-SEC-001 | All data in transit must use TLS 1.3 encryption | Must |
| NFR-SEC-002 | All data at rest must use AES-256 encryption | Must |
| NFR-SEC-003 | PHI must be stored in HIPAA-compliant infrastructure | Must |
| NFR-SEC-004 | System shall implement RBAC | Must |
| NFR-SEC-005 | System shall log all data access | Must |
| NFR-SEC-006 | System shall support SOC 2 Type II audit | Must |
| NFR-SEC-007 | Penetration testing shall be conducted quarterly | Must |
| NFR-SEC-008 | System shall implement rate limiting | Must |
| NFR-SEC-009 | Multi-factor authentication for admin access | Must |

### 5.3 Reliability Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-REL-001 | System uptime | 99.9% |
| NFR-REL-002 | Mean Time to Recovery (MTTR) | < 1 hour |
| NFR-REL-003 | Data backup frequency | Every 6 hours |
| NFR-REL-004 | Disaster recovery time | < 4 hours |
| NFR-REL-005 | Data retention period | 7 years (configurable) |

### 5.4 Scalability Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-SCL-001 | Horizontal scaling capability | Auto-scale 2x-10x |
| NFR-SCL-002 | Database scaling | Sharding support |
| NFR-SCL-003 | Geographic distribution | Multi-region |
| NFR-SCL-004 | Peak load handling | 5x normal traffic |

### 5.5 Usability Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-USE-001 | Time to complete onboarding | < 5 minutes |
| NFR-USE-002 | Daily check-in time | < 2 minutes |
| NFR-USE-003 | System Usability Scale (SUS) score | > 75 |
| NFR-USE-004 | Accessibility compliance | WCAG 2.1 AA |
| NFR-USE-005 | Language support | English (MVP) |

### 5.6 Compliance Requirements

| ID | Requirement | Standard |
|----|-------------|----------|
| NFR-CMP-001 | GDPR compliance | EU 2016/679 |
| NFR-CMP-002 | HIPAA compliance | 45 CFR Part 160, 164 |
| NFR-CMP-003 | SOC 2 Type II certification | AICPA |
| NFR-CMP-004 | ISO 27001 certification | ISO/IEC 27001 |
| NFR-CMP-005 | Data residency requirements | Configurable per region |

---

## 6. Data Requirements

### 6.1 Data Entities

| Entity | Description | Retention |
|--------|-------------|-----------|
| User | User account and preferences | Account lifetime + 30 days |
| Consent | Data collection consent records | 7 years |
| TextEntry | Journal entries (processed, not raw) | 2 years |
| VoiceFeatures | Prosodic feature vectors | 2 years |
| BehavioralData | Aggregated activity metrics | 2 years |
| RiskScore | Historical risk assessments | 7 years |
| Recommendation | Intervention suggestions made | 7 years |
| AuditLog | All system access logs | 7 years |

### 6.2 Data Flow Requirements

| ID | Requirement |
|----|-------------|
| DFR-001 | Raw text shall be processed and discarded within 24 hours |
| DFR-002 | Raw audio shall never leave the user's device |
| DFR-003 | Personal identifiers shall be stored separately from health data |
| DFR-004 | All data exports shall be anonymized using k-anonymity (k≥20) |
| DFR-005 | Data deletion requests shall be processed within 72 hours |

---

## 7. Appendix

### 7.1 Requirement Traceability Matrix

| BRD Objective | SRS Requirements |
|---------------|-----------------|
| BO-01 (Time to intervention) | FR-RSK-*, FR-REC-* |
| BO-02 (Well-being improvement) | FR-NLP-*, FR-VOC-*, FR-BHV-* |
| BO-03 (Turnover reduction) | All features |
| BO-04 (Resource efficiency) | FR-CLI-*, FR-REC-* |
| BO-05 (User adoption) | FR-APP-*, NFR-USE-* |
| BO-06 (User trust) | NFR-SEC-*, NFR-CMP-* |

### 7.2 Open Issues

| Issue ID | Description | Owner | Due Date |
|----------|-------------|-------|----------|
| OI-001 | Finalize voice model accuracy targets | Data Science | TBD |
| OI-002 | Confirm EHR integration scope | Product | TBD |
| OI-003 | Define language support roadmap | Product | TBD |

---

*Document Status: Draft - Pending Review*
