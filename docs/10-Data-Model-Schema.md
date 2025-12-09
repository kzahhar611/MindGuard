# Data Model & Database Schema
## MindGuard - AI Mental Health Early Detection System

---

## 1. Database Architecture

| Database | Type | Purpose |
|----------|------|---------|
| **PostgreSQL** | Relational | Users, consents, audit logs |
| **MongoDB** | Document | Health data, features, risk scores |
| **Redis** | Cache | Sessions, rate limiting, temp data |

---

## 2. PostgreSQL Schema

### 2.1 Users Table
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    organization_id UUID REFERENCES organizations(id),
    status VARCHAR(20) DEFAULT 'active',
    mfa_enabled BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login_at TIMESTAMP
);
```

### 2.2 Organizations Table
```sql
CREATE TABLE organizations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    domain VARCHAR(255),
    subscription_tier VARCHAR(50),
    settings JSONB DEFAULT '{}',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 2.3 Consents Table
```sql
CREATE TABLE consents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id),
    consent_type VARCHAR(50) NOT NULL,
    granted BOOLEAN NOT NULL,
    version VARCHAR(20) NOT NULL,
    ip_address VARCHAR(45),
    user_agent TEXT,
    granted_at TIMESTAMP,
    revoked_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, consent_type)
);
```

### 2.4 Audit Logs Table
```sql
CREATE TABLE audit_logs (
    id BIGSERIAL PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    action VARCHAR(50) NOT NULL,
    resource_type VARCHAR(50),
    resource_id UUID,
    ip_address VARCHAR(45),
    user_agent TEXT,
    metadata JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_audit_user ON audit_logs(user_id, created_at);
CREATE INDEX idx_audit_action ON audit_logs(action, created_at);
```

---

## 3. MongoDB Collections

### 3.1 Journal Entries
```javascript
// journals collection
{
  "_id": ObjectId,
  "user_id": "uuid",
  "content_encrypted": "encrypted_string",
  "word_count": 156,
  "prompt_id": "uuid_or_null",
  "analysis_status": "pending|completed|failed",
  "created_at": ISODate,
  "updated_at": ISODate
}
```

### 3.2 Text Analysis Features
```javascript
// text_features collection
{
  "_id": ObjectId,
  "user_id": "uuid",
  "journal_id": "uuid",
  "sentiment": {
    "label": "positive",
    "score": 0.85,
    "valence": 0.6
  },
  "cognitive_distortions": {
    "detected": ["all_or_nothing"],
    "count": 2
  },
  "vocabulary": {
    "complexity_score": 65,
    "unique_ratio": 0.72
  },
  "stress_score": 35,
  "risk_contribution": 42,
  "model_version": "nlp-v1.0.0",
  "created_at": ISODate
}
```

### 3.3 Voice Features
```javascript
// voice_features collection
{
  "_id": ObjectId,
  "user_id": "uuid",
  "duration_seconds": 45,
  "prosody": {
    "pace": "normal",
    "pitch_variability": 35,
    "pause_frequency": "low"
  },
  "emotion": {
    "primary": "calm",
    "confidence": 0.78
  },
  "stress_score": 42,
  "risk_contribution": 45,
  "model_version": "voice-v1.0.0",
  "device_model": "iPhone 15",
  "created_at": ISODate
}
```

### 3.4 Behavioral Data
```javascript
// behavioral_data collection
{
  "_id": ObjectId,
  "user_id": "uuid",
  "date": "YYYY-MM-DD",
  "sleep": {
    "duration_hours": 6.5,
    "start_time": "23:30",
    "end_time": "06:00",
    "quality_score": 72
  },
  "activity": {
    "steps": 4500,
    "active_minutes": 25,
    "workouts": []
  },
  "baseline_comparison": {
    "sleep_deviation": -12,
    "activity_deviation": -25
  },
  "anomaly_flags": ["low_activity"],
  "risk_contribution": 55,
  "source": "apple_healthkit",
  "created_at": ISODate
}
```

### 3.5 Risk Scores
```javascript
// risk_scores collection
{
  "_id": ObjectId,
  "user_id": "uuid",
  "score": 52,
  "level": "moderate",
  "confidence": 0.85,
  "components": {
    "text": 42,
    "voice": 48,
    "behavioral": 55
  },
  "weights_used": {
    "text": 0.4,
    "voice": 0.3,
    "behavioral": 0.3
  },
  "trend": "stable",
  "trend_7d": [48, 50, 51, 52, 52, 51, 52],
  "calculated_at": ISODate,
  "valid_until": ISODate
}
```

### 3.6 Recommendations
```javascript
// recommendations collection
{
  "_id": ObjectId,
  "user_id": "uuid",
  "type": "exercise|article|coaching|clinical",
  "resource_id": "uuid",
  "priority": 1,
  "reason": "elevated_stress",
  "status": "pending|viewed|completed|dismissed",
  "presented_at": ISODate,
  "completed_at": ISODate
}
```

---

## 4. Entity Relationship Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              ENTITY RELATIONSHIPS                                │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│   ┌──────────────┐       ┌──────────────┐       ┌──────────────┐                │
│   │ Organization │◄──────│    User      │──────►│   Consents   │                │
│   │              │ 1:N   │              │ 1:N   │              │                │
│   └──────────────┘       └──────┬───────┘       └──────────────┘                │
│                                 │                                                │
│                                 │ 1:N                                            │
│              ┌──────────────────┼──────────────────┐                            │
│              │                  │                  │                            │
│              ▼                  ▼                  ▼                            │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐                       │
│   │   Journals   │   │    Voice     │   │  Behavioral  │                       │
│   │   (MongoDB)  │   │   Features   │   │    Data      │                       │
│   └──────┬───────┘   │   (MongoDB)  │   │   (MongoDB)  │                       │
│          │           └──────────────┘   └──────────────┘                       │
│          │ 1:1                                                                   │
│          ▼                                                                       │
│   ┌──────────────┐                                                              │
│   │ Text Features│                                                              │
│   │  (MongoDB)   │                                                              │
│   └──────────────┘                                                              │
│                                                                                  │
│              └──────────────────┬──────────────────┘                            │
│                                 │                                                │
│                                 ▼                                                │
│                      ┌──────────────────┐                                       │
│                      │   Risk Scores    │                                       │
│                      │    (MongoDB)     │                                       │
│                      └────────┬─────────┘                                       │
│                               │ 1:N                                              │
│                               ▼                                                  │
│                      ┌──────────────────┐                                       │
│                      │  Recommendations │                                       │
│                      │    (MongoDB)     │                                       │
│                      └──────────────────┘                                       │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Indexes

### PostgreSQL
```sql
-- Performance indexes
CREATE INDEX idx_users_org ON users(organization_id);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_consents_user ON consents(user_id);
CREATE INDEX idx_audit_user_time ON audit_logs(user_id, created_at DESC);
```

### MongoDB
```javascript
// journals
db.journals.createIndex({ user_id: 1, created_at: -1 });

// text_features
db.text_features.createIndex({ user_id: 1, created_at: -1 });
db.text_features.createIndex({ journal_id: 1 });

// risk_scores
db.risk_scores.createIndex({ user_id: 1, calculated_at: -1 });

// behavioral_data
db.behavioral_data.createIndex({ user_id: 1, date: -1 });
```

---

## 6. Data Encryption

| Field Type | Encryption |
|------------|------------|
| Journal content | Application-level AES-256 |
| Email | Field-level encryption |
| Password | bcrypt hash |
| Feature vectors | DB-level TDE |

---

*Document Status: Draft*
