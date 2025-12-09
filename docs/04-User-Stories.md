# User Stories Document
## AI-Driven Mental Health Early Detection System
### Project Codename: "MindGuard"

---

## Document Control

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2024-12-09 | Development Team | Initial User Stories |

---

## 1. User Personas Summary

| Persona | Description | Primary Goals |
|---------|-------------|---------------|
| **Alex** | Overwhelmed Professional | Early burnout detection, quick self-help |
| **Jordan** | HR Wellness Champion | Aggregate insights, adoption metrics |
| **Dr. Maya** | Corporate Therapist | Triage, patient prioritization |
| **Sam** | System Administrator | System health, security, compliance |

---

## 2. Epic Overview

| Epic ID | Epic Name | Description |
|---------|-----------|-------------|
| EP-01 | User Onboarding & Consent | Registration, consent management, initial setup |
| EP-02 | Text-Based Analysis | Journaling, text submission, NLP insights |
| EP-03 | Voice Analysis | Voice check-in, prosody-based insights |
| EP-04 | Behavioral Tracking | Wearable integration, pattern detection |
| EP-05 | Risk Assessment | Multi-modal risk scoring, visualization |
| EP-06 | Recommendations | Personalized interventions, resources |
| EP-07 | Admin Dashboard | Organizational analytics, compliance |
| EP-08 | Clinical Portal | Triage, patient management |
| EP-09 | Privacy & Data Control | Data export, deletion, transparency |

---

## 3. User Stories by Epic

### EP-01: User Onboarding & Consent

#### US-001: User Registration
**As** a new user (Alex)  
**I want to** create an account using my email or SSO  
**So that** I can access the mental health monitoring features

**Acceptance Criteria:**
- [ ] User can register with email/password
- [ ] User can register via Google/Apple/Microsoft SSO
- [ ] Email verification is required before account activation
- [ ] Password must meet complexity requirements (8+ chars, upper, lower, number)
- [ ] Registration takes less than 2 minutes

**Story Points:** 5  
**Priority:** Must Have

---

#### US-002: Consent Presentation
**As** a new user (Alex)  
**I want to** clearly understand what data will be collected and how it will be used  
**So that** I can make an informed decision about participating

**Acceptance Criteria:**
- [ ] Consent form is displayed in plain language (reading level: Grade 8)
- [ ] Each data stream (text, voice, behavioral) has separate opt-in toggles
- [ ] Privacy policy and terms of service are accessible
- [ ] User must actively agree (no pre-checked boxes)
- [ ] Consent version is recorded with timestamp

**Story Points:** 3  
**Priority:** Must Have

---

#### US-003: Granular Consent Management
**As** an existing user (Alex)  
**I want to** change my data collection preferences at any time  
**So that** I remain in control of my personal data

**Acceptance Criteria:**
- [ ] User can access consent settings from profile
- [ ] Each data stream can be independently enabled/disabled
- [ ] Changes take effect immediately
- [ ] Historical consent changes are logged
- [ ] User receives confirmation of changes

**Story Points:** 3  
**Priority:** Must Have

---

#### US-004: Initial Baseline Assessment
**As** a new user (Alex)  
**I want to** complete an initial self-assessment questionnaire  
**So that** the system can establish my personal baseline

**Acceptance Criteria:**
- [ ] PHQ-9 and GAD-7 questionnaires are presented (optional, not diagnostic)
- [ ] Assessment takes less than 10 minutes
- [ ] User can skip and return later
- [ ] Results are shown with context (not clinical diagnosis)
- [ ] Baseline is used to calibrate future risk scores

**Story Points:** 5  
**Priority:** Should Have

---

### EP-02: Text-Based Analysis

#### US-005: Free-Form Journaling
**As** a user (Alex)  
**I want to** write journal entries about my thoughts and feelings  
**So that** I can reflect and the system can analyze my emotional state

**Acceptance Criteria:**
- [ ] Journal editor supports free-form text entry
- [ ] Auto-save every 30 seconds
- [ ] Entries can be saved as draft or completed
- [ ] Word count is displayed
- [ ] Entries are stored locally for offline use

**Story Points:** 5  
**Priority:** Must Have

---

#### US-006: Guided Journal Prompts
**As** a user (Alex)  
**I want to** receive contextual prompts to guide my journaling  
**So that** I can overcome writer's block and structure my thoughts

**Acceptance Criteria:**
- [ ] System suggests 3-5 prompts based on risk level
- [ ] Prompts change daily
- [ ] User can dismiss prompts and write freely
- [ ] Prompts are evidence-based (CBT/DBT inspired)
- [ ] Users can save favorite prompts

**Story Points:** 3  
**Priority:** Should Have

---

#### US-007: Text Sentiment Insights
**As** a user (Alex)  
**I want to** see insights about my writing patterns over time  
**So that** I can understand my emotional trends

**Acceptance Criteria:**
- [ ] Sentiment trend graph (week/month view)
- [ ] Vocabulary complexity indicator
- [ ] Cognitive distortion flags (displayed gently, not alarmingly)
- [ ] Comparison to personal baseline
- [ ] Educational context for each insight

**Story Points:** 8  
**Priority:** Should Have

---

### EP-03: Voice Analysis

#### US-008: Voice Check-In Recording
**As** a user (Alex)  
**I want to** record a short voice sample as part of my check-in  
**So that** the system can analyze my emotional state from my voice

**Acceptance Criteria:**
- [ ] One-tap to start recording
- [ ] Guided prompt displayed (e.g., "Tell us about your day")
- [ ] Recording duration: 30-90 seconds
- [ ] Audio quality indicator
- [ ] Clear indicator that audio content is NOT stored

**Story Points:** 5  
**Priority:** Must Have

---

#### US-009: On-Device Voice Processing
**As** a privacy-conscious user (Alex)  
**I want to** have my voice processed on my device  
**So that** my actual words are never transmitted or stored

**Acceptance Criteria:**
- [ ] Prosodic features are extracted on-device
- [ ] Raw audio is deleted immediately after processing
- [ ] Only numerical feature vectors are transmitted (encrypted)
- [ ] Processing completes within 5 seconds
- [ ] Clear privacy indicator shown during processing

**Story Points:** 13  
**Priority:** Must Have

---

#### US-010: Voice Trend Visualization
**As** a user (Alex)  
**I want to** see how my voice patterns have changed over time  
**So that** I can correlate with my subjective feelings

**Acceptance Criteria:**
- [ ] Graph showing voice-derived stress indicator over time
- [ ] Annotations for significant changes
- [ ] Correlation with journaling sentiment (if enabled)
- [ ] Non-clinical language used ("stress indicator", not "depression score")

**Story Points:** 5  
**Priority:** Should Have

---

### EP-04: Behavioral Tracking

#### US-011: Wearable Device Connection
**As** a user (Alex)  
**I want to** connect my fitness tracker/smartwatch to the app  
**So that** the system can analyze my activity and sleep patterns

**Acceptance Criteria:**
- [ ] Apple HealthKit integration for iOS
- [ ] Google Fit integration for Android
- [ ] Clear permissions request with explanation
- [ ] Manual disconnect option in settings
- [ ] Data sync indicator

**Story Points:** 8  
**Priority:** Must Have

---

#### US-012: Sleep Pattern Analysis
**As** a user (Alex)  
**I want to** see insights about my sleep patterns  
**So that** I can understand how sleep affects my well-being

**Acceptance Criteria:**
- [ ] Average sleep duration displayed
- [ ] Sleep timing consistency indicator
- [ ] Comparison to personal baseline
- [ ] Anomaly alerts (gentle, not alarming)
- [ ] Educational tips for sleep hygiene

**Story Points:** 5  
**Priority:** Should Have

---

#### US-013: Activity Pattern Analysis
**As** a user (Alex)  
**I want to** see insights about my physical activity patterns  
**So that** I can understand how activity affects my mood

**Acceptance Criteria:**
- [ ] Daily step count and trends
- [ ] Active minutes comparison to baseline
- [ ] Sedentary time alerts (optional)
- [ ] Correlation with risk score (educational)

**Story Points:** 5  
**Priority:** Should Have

---

### EP-05: Risk Assessment

#### US-014: Personal Risk Dashboard
**As** a user (Alex)  
**I want to** see my current well-being status at a glance  
**So that** I know when to seek support or practice self-care

**Acceptance Criteria:**
- [ ] Risk level displayed with color coding (green/yellow/orange/red)
- [ ] Trend indicator (improving/stable/declining)
- [ ] Last updated timestamp
- [ ] Breakdown by contributing factors (text, voice, behavior)
- [ ] Non-clinical language throughout

**Story Points:** 8  
**Priority:** Must Have

---

#### US-015: Risk Alert Notifications
**As** a user (Alex)  
**I want to** receive notifications when my risk level changes significantly  
**So that** I can take action before things get worse

**Acceptance Criteria:**
- [ ] Push notification on significant risk increase (configurable)
- [ ] Notification includes suggested action
- [ ] User can disable/configure notification preferences
- [ ] Notifications are never visible to lock screen (privacy)
- [ ] Crisis resources always accessible from notification

**Story Points:** 5  
**Priority:** Must Have

---

#### US-016: Historical Risk Trend
**As** a user (Alex)  
**I want to** see my risk history over time  
**So that** I can identify patterns and track my progress

**Acceptance Criteria:**
- [ ] Interactive graph (1 week, 1 month, 3 months, 1 year views)
- [ ] Annotations for significant events (journaling, coaching)
- [ ] Export to PDF option
- [ ] Baseline comparison line

**Story Points:** 5  
**Priority:** Should Have

---

### EP-06: Recommendations

#### US-017: Personalized Self-Help Resources
**As** a user (Alex)  
**I want to** receive recommendations based on my current risk level  
**So that** I can take appropriate action to improve my well-being

**Acceptance Criteria:**
- [ ] Recommendations change based on risk level
- [ ] Mix of breathing exercises, articles, videos
- [ ] Track engagement with recommendations
- [ ] "Not helpful" feedback option
- [ ] Save favorites for quick access

**Story Points:** 8  
**Priority:** Must Have

---

#### US-018: Guided Breathing/Meditation Exercises
**As** a user (Alex)  
**I want to** access quick breathing and mindfulness exercises  
**So that** I can manage acute stress in the moment

**Acceptance Criteria:**
- [ ] Minimum 3 breathing exercises available
- [ ] Minimum 3 guided meditations (2-10 minutes)
- [ ] Visual/audio guidance options
- [ ] Track completion
- [ ] Offline access

**Story Points:** 5  
**Priority:** Must Have

---

#### US-019: Coaching Session Booking
**As** a user with elevated risk (Alex)  
**I want to** easily book a session with a wellness coach  
**So that** I can get professional support before issues escalate

**Acceptance Criteria:**
- [ ] Coach availability calendar displayed
- [ ] One-click booking
- [ ] Confirmation and reminder notifications
- [ ] Rescheduling/cancellation options
- [ ] Pre-session summary shared with coach (with consent)

**Story Points:** 8  
**Priority:** Should Have

---

#### US-020: Crisis Resource Access
**As** a user in crisis  
**I want to** quickly access emergency resources  
**So that** I can get immediate help

**Acceptance Criteria:**
- [ ] Crisis button accessible from any screen
- [ ] National crisis hotline numbers displayed
- [ ] Local emergency services option
- [ ] Available without login
- [ ] Simplified, calming UI

**Story Points:** 3  
**Priority:** Must Have

---

### EP-07: Admin Dashboard

#### US-021: Organization Well-Being Overview
**As** an HR wellness admin (Jordan)  
**I want to** see aggregate well-being metrics for my organization  
**So that** I can identify concerning trends and measure program effectiveness

**Acceptance Criteria:**
- [ ] Average well-being score (anonymized)
- [ ] Distribution graph (% in each risk category)
- [ ] Trend over time (month/quarter/year)
- [ ] No individual-level data visible
- [ ] Minimum group size of 20 for any view

**Story Points:** 8  
**Priority:** Must Have

---

#### US-022: Adoption Metrics Dashboard
**As** an HR wellness admin (Jordan)  
**I want to** see adoption and engagement metrics  
**So that** I can drive program participation

**Acceptance Criteria:**
- [ ] Enrollment rate (registered / invited)
- [ ] Active user rate (active in last 30 days / enrolled)
- [ ] Feature usage breakdown (journaling, voice, resources)
- [ ] Trend over time
- [ ] Export to CSV

**Story Points:** 5  
**Priority:** Must Have

---

#### US-023: Department Comparison
**As** an HR wellness admin (Jordan)  
**I want to** compare well-being metrics across departments  
**So that** I can identify areas needing targeted intervention

**Acceptance Criteria:**
- [ ] Only if department size > 20 (k-anonymity)
- [ ] Comparison chart with confidence intervals
- [ ] No individual identification possible
- [ ] Drill-down disabled for small groups

**Story Points:** 5  
**Priority:** Should Have

---

#### US-024: Compliance Reporting
**As** an HR wellness admin (Jordan)  
**I want to** generate compliance reports  
**So that** I can demonstrate GDPR/HIPAA compliance to auditors

**Acceptance Criteria:**
- [ ] Consent summary report
- [ ] Data access log summary
- [ ] Data deletion request summary
- [ ] Export to PDF with audit trail
- [ ] Scheduled report option

**Story Points:** 5  
**Priority:** Must Have

---

### EP-08: Clinical Portal

#### US-025: Risk-Ranked Patient Queue
**As** a clinician (Dr. Maya)  
**I want to** see my patients ranked by risk level  
**So that** I can prioritize those who need the most urgent attention

**Acceptance Criteria:**
- [ ] Patient list sorted by risk score (highest first)
- [ ] Risk level color coding
- [ ] Last check-in date
- [ ] Quick view of risk trend
- [ ] Consent status indicator

**Story Points:** 8  
**Priority:** Must Have

---

#### US-026: Patient Risk Summary
**As** a clinician (Dr. Maya)  
**I want to** see a detailed risk summary for a patient  
**So that** I can prepare for our session

**Acceptance Criteria:**
- [ ] Overall risk score and trend
- [ ] Component breakdown (text, voice, behavioral)
- [ ] Recent journal themes (if consented)
- [ ] Recommended talking points
- [ ] Patient preferences/notes

**Story Points:** 8  
**Priority:** Must Have

---

#### US-027: Session Notes
**As** a clinician (Dr. Maya)  
**I want to** add session notes linked to a patient's profile  
**So that** I can track progress and maintain continuity

**Acceptance Criteria:**
- [ ] Rich text editor for notes
- [ ] Notes are separate from AI-generated data
- [ ] Encrypted storage
- [ ] Accessible only by assigned clinician
- [ ] Export for EHR integration (Phase 3)

**Story Points:** 5  
**Priority:** Should Have

---

### EP-09: Privacy & Data Control

#### US-028: Data Export (GDPR)
**As** a user (Alex)  
**I want to** export all my personal data  
**So that** I can see what the system knows about me

**Acceptance Criteria:**
- [ ] One-click data export request
- [ ] Data delivered within 48 hours
- [ ] Includes all data categories
- [ ] Machine-readable format (JSON)
- [ ] Email notification when ready

**Story Points:** 5  
**Priority:** Must Have

---

#### US-029: Account Deletion (Right to be Forgotten)
**As** a user (Alex)  
**I want to** permanently delete my account and all data  
**So that** I can exercise my right to be forgotten

**Acceptance Criteria:**
- [ ] Clear deletion request flow
- [ ] Confirmation required (prevent accidental deletion)
- [ ] Data deleted within 72 hours
- [ ] Email confirmation of deletion
- [ ] Anonymized aggregate data may be retained (disclosed)

**Story Points:** 5  
**Priority:** Must Have

---

#### US-030: Privacy Dashboard
**As** a user (Alex)  
**I want to** see a summary of my data and how it's used  
**So that** I can maintain transparency and trust

**Acceptance Criteria:**
- [ ] List of data categories collected
- [ ] Last access log
- [ ] Active consents summary
- [ ] Quick links to change settings
- [ ] Clear explanations

**Story Points:** 3  
**Priority:** Must Have

---

## 4. User Story Backlog Summary

| Epic | Total Stories | Story Points | Priority Distribution |
|------|---------------|--------------|----------------------|
| EP-01: Onboarding | 4 | 16 | 3 Must, 1 Should |
| EP-02: Text Analysis | 3 | 16 | 1 Must, 2 Should |
| EP-03: Voice Analysis | 3 | 23 | 2 Must, 1 Should |
| EP-04: Behavioral | 3 | 18 | 1 Must, 2 Should |
| EP-05: Risk Assessment | 3 | 18 | 2 Must, 1 Should |
| EP-06: Recommendations | 4 | 24 | 2 Must, 2 Should |
| EP-07: Admin Dashboard | 4 | 23 | 2 Must, 2 Should |
| EP-08: Clinical Portal | 3 | 21 | 2 Must, 1 Should |
| EP-09: Privacy | 3 | 13 | 3 Must |
| **TOTAL** | **30** | **172** | **18 Must, 12 Should** |

---

## 5. Story Dependency Map

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         USER STORY DEPENDENCIES                                   │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                   │
│   US-001 (Registration)                                                           │
│      │                                                                            │
│      └──► US-002 (Consent) ──► US-003 (Consent Management)                       │
│              │                                                                    │
│              │                                                                    │
│              ├──► US-005 (Journaling) ──► US-006 (Prompts) ──► US-007 (Insights) │
│              │                                                                    │
│              ├──► US-008 (Voice Recording) ──► US-009 (Edge Processing)          │
│              │                                   │                                │
│              │                                   └──► US-010 (Voice Trends)       │
│              │                                                                    │
│              └──► US-011 (Wearables) ──► US-012 (Sleep) ──► US-013 (Activity)    │
│                                                                                   │
│                                        │                                          │
│                                        ▼                                          │
│                           ┌───────────────────────┐                               │
│                           │ US-014 (Risk Dashboard)│                              │
│                           └───────────┬───────────┘                               │
│                                       │                                           │
│              ┌────────────────────────┼────────────────────────┐                 │
│              │                        │                        │                  │
│              ▼                        ▼                        ▼                  │
│   US-015 (Alerts)       US-016 (History)       US-017 (Recommendations)          │
│                                                        │                          │
│                                        ┌───────────────┼───────────────┐         │
│                                        │               │               │         │
│                                        ▼               ▼               ▼         │
│                                   US-018          US-019          US-020         │
│                                  (Exercises)     (Coaching)      (Crisis)        │
│                                                                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

*Document Status: Draft - Pending Review*
