# Privacy & Compliance Documentation
## MindGuard - AI Mental Health Early Detection System

---

## 1. Regulatory Compliance Overview

| Regulation | Jurisdiction | Status |
|------------|--------------|--------|
| **GDPR** | European Union | Required |
| **HIPAA** | United States | Required |
| **CCPA** | California, USA | Required |
| **SOC 2 Type II** | Global | Targeted |
| **ISO 27001** | Global | Targeted |

---

## 2. Data Classification

| Level | Description | Examples | Handling |
|-------|-------------|----------|----------|
| **Critical** | Health/PHI data | Risk scores, analysis | Encrypted, audit logged |
| **Sensitive** | Personal identifiers | Email, name | Encrypted, access controlled |
| **Internal** | Aggregated data | Org statistics | Anonymized, k=20 |
| **Public** | Marketing content | App screenshots | No restrictions |

---

## 3. GDPR Compliance

### 3.1 User Rights Implementation

| Right | Feature | SLA |
|-------|---------|-----|
| Right to Access | Data export | 48 hours |
| Right to Rectification | Profile edit | Immediate |
| Right to Erasure | Account deletion | 72 hours |
| Right to Data Portability | JSON export | 48 hours |
| Right to Withdraw Consent | Consent settings | Immediate |

### 3.2 Lawful Basis

| Data Processing | Basis | Justification |
|-----------------|-------|---------------|
| Risk scoring | Consent | Explicit opt-in |
| Text analysis | Consent | Explicit opt-in |
| Voice analysis | Consent | Explicit opt-in |
| Safety alerts | Legitimate interest | User wellbeing |
| Aggregate analytics | Legitimate interest | Service improvement |

### 3.3 Data Processing Records

All processing activities documented including:
- Purpose of processing
- Categories of data subjects
- Categories of personal data
- Recipients of data
- Retention periods
- Security measures

---

## 4. HIPAA Compliance

### 4.1 PHI Safeguards

| Safeguard | Implementation |
|-----------|----------------|
| **Administrative** | Security policies, training, access management |
| **Physical** | Data center security, device policies |
| **Technical** | Encryption, audit logs, access controls |

### 4.2 Technical Controls

| Control | Specification |
|---------|---------------|
| Encryption at rest | AES-256 |
| Encryption in transit | TLS 1.3 |
| Access logging | All PHI access logged |
| Minimum necessary | Role-based access |
| Unique user IDs | Individual accountability |
| Automatic logoff | 15-minute timeout |
| Audit controls | 7-year retention |

### 4.3 Business Associate Agreement

Required for all third-party processors:
- Cloud providers (Azure)
- Analytics services
- Customer support tools

---

## 5. Privacy by Design

### 5.1 Edge Processing for Voice

```
┌─────────────────────────────────────────────┐
│ VOICE PRIVACY ARCHITECTURE                   │
├─────────────────────────────────────────────┤
│                                              │
│   User Device           Cloud               │
│   ───────────           ─────               │
│   [Audio] ──►           ╳ No audio          │
│       │                                      │
│       ▼                                      │
│   [Feature             [Features Only]      │
│    Extract] ────────────────────────►       │
│       │                                      │
│       ▼                                      │
│   [Delete Audio]                             │
│                                              │
│   🔒 Words are NEVER transmitted            │
│                                              │
└─────────────────────────────────────────────┘
```

### 5.2 Data Minimization

| Data Type | Retention | Minimization |
|-----------|-----------|--------------|
| Raw text | 24 hours | Deleted after analysis |
| Raw audio | 0 (never stored) | Edge processing only |
| Features | 2 years | Only derived metrics |
| Risk scores | 7 years | Required for trends |

### 5.3 Anonymization

- **k-Anonymity**: Minimum group size of 20 for any aggregate view
- **Differential Privacy**: Noise added to aggregate statistics
- **Pseudonymization**: User IDs separated from PII

---

## 6. Data Retention

| Data Category | Retention Period | Justification |
|---------------|------------------|---------------|
| User profiles | Account lifetime + 30 days | Service delivery |
| Consent records | 7 years | Legal compliance |
| Health features | 2 years | Trend analysis |
| Risk scores | 7 years | Clinical continuity |
| Audit logs | 7 years | HIPAA requirement |
| Analytics | 3 years | Service improvement |

---

## 7. Security Measures

### 7.1 Access Control

| Level | Access | Approval |
|-------|--------|----------|
| User | Own data only | Self |
| Support | User data (with ticket) | Manager |
| Admin | Aggregate data only | CISO |
| Data Science | Anonymized data | DPO + CISO |

### 7.2 Encryption

| Layer | Algorithm | Key Management |
|-------|-----------|----------------|
| Data at rest | AES-256-GCM | Azure Key Vault |
| Data in transit | TLS 1.3 | Managed certificates |
| Database | TDE | Azure managed |
| Backups | AES-256 | Separate key |

### 7.3 Incident Response

| Severity | Response Time | Notification |
|----------|---------------|--------------|
| Critical (breach) | 1 hour | DPO within 24h |
| High | 4 hours | Internal 48h |
| Medium | 24 hours | Weekly review |
| Low | 72 hours | Monthly review |

---

## 8. Consent Management

### 8.1 Consent Types

| Type | Description | Revocable |
|------|-------------|-----------|
| Text Analysis | Journal/message analysis | Yes |
| Voice Analysis | Prosodic feature extraction | Yes |
| Behavioral Data | Wearable integration | Yes |
| Notifications | Push notifications | Yes |
| Analytics | Anonymous usage data | Yes |

### 8.2 Consent UI Requirements

- [ ] Plain language (Grade 8 reading level)
- [ ] Separate toggles per data stream
- [ ] No pre-checked boxes
- [ ] Easy to revoke
- [ ] Version tracking
- [ ] Timestamp logging

---

## 9. Third-Party Data Sharing

### 9.1 Data Recipients

| Recipient | Data Shared | Purpose | DPA |
|-----------|-------------|---------|-----|
| Azure | All (encrypted) | Hosting | Yes |
| Firebase | Device tokens | Push notifications | Yes |
| None | Individual data | N/A | N/A |

### 9.2 Never Shared

❌ Individual risk scores with employers
❌ Personal data for marketing
❌ Data with insurance companies
❌ Data for performance reviews

---

## 10. Audit & Monitoring

### 10.1 Audit Log Contents

| Event | Fields Logged |
|-------|---------------|
| Login | User ID, timestamp, IP, device |
| Data access | User ID, resource, action, timestamp |
| Consent change | User ID, type, value, timestamp |
| Data export | User ID, timestamp, format |
| Account deletion | User ID, timestamp, confirmation |

### 10.2 Monitoring

| Check | Frequency | Alert |
|-------|-----------|-------|
| Unusual access patterns | Real-time | Yes |
| Failed login attempts | Real-time | Yes (>5) |
| Data export volume | Daily | Yes (>threshold) |
| Compliance drift | Weekly | Yes |

---

*Document Status: Draft*
