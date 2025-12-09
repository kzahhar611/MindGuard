# System Architecture Document
## AI-Driven Mental Health Early Detection System
### Project Codename: "MindGuard"

---

## Document Control

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2024-12-09 | Development Team | Initial Architecture |

---

## 1. Architecture Overview

### 1.1 High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│                          MINDGUARD SYSTEM ARCHITECTURE                                 │
├──────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                        │
│   ╔═══════════════════════════════════════════════════════════════════════════════╗   │
│   ║                            CLIENT LAYER                                        ║   │
│   ╠═══════════════════════════════════════════════════════════════════════════════╣   │
│   ║  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐                ║   │
│   ║  │   iOS App       │  │   Android App   │  │   Web Dashboard │                ║   │
│   ║  │   (Swift)       │  │   (Kotlin)      │  │   (React.js)    │                ║   │
│   ║  │                 │  │                 │  │                 │                ║   │
│   ║  │ ┌─────────────┐ │  │ ┌─────────────┐ │  │ • Admin Portal  │                ║   │
│   ║  │ │ Edge AI SDK │ │  │ │ Edge AI SDK │ │  │ • Clinical View │                ║   │
│   ║  │ │ (CoreML)    │ │  │ │ (TF Lite)   │ │  │ • Analytics     │                ║   │
│   ║  │ └─────────────┘ │  │ └─────────────┘ │  │                 │                ║   │
│   ║  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘                ║   │
│   ╚═══════════╪════════════════════╪════════════════════╪═════════════════════════╝   │
│               │                    │                    │                              │
│               └────────────────────┼────────────────────┘                              │
│                                    │ HTTPS / WSS                                       │
│   ╔════════════════════════════════╪══════════════════════════════════════════════╗   │
│   ║                      API GATEWAY LAYER                                         ║   │
│   ╠════════════════════════════════╪══════════════════════════════════════════════╣   │
│   ║                 ┌──────────────▼──────────────┐                                ║   │
│   ║                 │      API Gateway            │                                ║   │
│   ║                 │      (Kong / AWS API GW)    │                                ║   │
│   ║                 │                             │                                ║   │
│   ║                 │ • Rate Limiting             │                                ║   │
│   ║                 │ • Authentication            │                                ║   │
│   ║                 │ • Request Routing           │                                ║   │
│   ║                 │ • SSL Termination           │                                ║   │
│   ║                 └──────────────┬──────────────┘                                ║   │
│   ╚════════════════════════════════╪══════════════════════════════════════════════╝   │
│                                    │                                                   │
│   ╔════════════════════════════════╪══════════════════════════════════════════════╗   │
│   ║                      MICROSERVICES LAYER                                       ║   │
│   ╠════════════════════════════════╪══════════════════════════════════════════════╣   │
│   ║                                │                                               ║   │
│   ║    ┌───────────────────────────┼───────────────────────────────┐              ║   │
│   ║    │                           │                               │              ║   │
│   ║    ▼                           ▼                               ▼              ║   │
│   ║  ┌─────────────┐  ┌─────────────────────┐  ┌─────────────────────┐           ║   │
│   ║  │ Auth        │  │ User Service        │  │ Consent Service     │           ║   │
│   ║  │ Service     │  │                     │  │                     │           ║   │
│   ║  │ ─────────── │  │ • Profile Mgmt      │  │ • Consent Tracking  │           ║   │
│   ║  │ • JWT       │  │ • Preferences       │  │ • Audit Logging     │           ║   │
│   ║  │ • OAuth 2.0 │  │ • Settings          │  │ • GDPR Compliance   │           ║   │
│   ║  │ • MFA       │  │                     │  │                     │           ║   │
│   ║  └─────────────┘  └─────────────────────┘  └─────────────────────┘           ║   │
│   ║                                                                               ║   │
│   ║  ┌─────────────────────────────────────────────────────────────────┐         ║   │
│   ║  │                    DATA INGESTION SERVICES                       │         ║   │
│   ║  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │         ║   │
│   ║  │  │ Text        │  │ Voice       │  │ Behavioral  │              │         ║   │
│   ║  │  │ Ingestion   │  │ Features    │  │ Data        │              │         ║   │
│   ║  │  │ Service     │  │ Ingestion   │  │ Ingestion   │              │         ║   │
│   ║  │  └─────────────┘  └─────────────┘  └─────────────┘              │         ║   │
│   ║  └─────────────────────────────────────────────────────────────────┘         ║   │
│   ║                                                                               ║   │
│   ║  ┌─────────────────────────────────────────────────────────────────┐         ║   │
│   ║  │                    ANALYSIS SERVICES                             │         ║   │
│   ║  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │         ║   │
│   ║  │  │ Risk        │  │ Recommend   │  │ Notification│              │         ║   │
│   ║  │  │ Scoring     │  │ Engine      │  │ Service     │              │         ║   │
│   ║  │  │ Service     │  │ Service     │  │             │              │         ║   │
│   ║  │  └─────────────┘  └─────────────┘  └─────────────┘              │         ║   │
│   ║  └─────────────────────────────────────────────────────────────────┘         ║   │
│   ║                                                                               ║   │
│   ╚═══════════════════════════════════════════════════════════════════════════════╝   │
│                                    │                                                   │
│   ╔════════════════════════════════╪══════════════════════════════════════════════╗   │
│   ║                      AI/ML LAYER                                               ║   │
│   ╠════════════════════════════════╪══════════════════════════════════════════════╣   │
│   ║  ┌─────────────────────────────┼───────────────────────────────┐              ║   │
│   ║  │                             │                               │              ║   │
│   ║  ▼                             ▼                               ▼              ║   │
│   ║  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐               ║   │
│   ║  │ NLP Pipeline    │  │ Voice Analysis  │  │ Anomaly         │               ║   │
│   ║  │ (Azure ML)      │  │ Pipeline        │  │ Detection       │               ║   │
│   ║  │                 │  │ (Azure ML)      │  │ Pipeline        │               ║   │
│   ║  │ • BERT/RoBERTa  │  │                 │  │ (Azure ML)      │               ║   │
│   ║  │ • Sentiment     │  │ • Prosody ML    │  │                 │               ║   │
│   ║  │ • Distortions   │  │ • Emotion Class │  │ • Isolation For │               ║   │
│   ║  │                 │  │                 │  │ • LSTM Baseline │               ║   │
│   ║  └─────────────────┘  └─────────────────┘  └─────────────────┘               ║   │
│   ║                                                                               ║   │
│   ║  ┌─────────────────────────────────────────────────────────────────┐         ║   │
│   ║  │                    MODEL MANAGEMENT                              │         ║   │
│   ║  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │         ║   │
│   ║  │  │ MLflow      │  │ Feature     │  │ Model       │              │         ║   │
│   ║  │  │ Registry    │  │ Store       │  │ Monitor     │              │         ║   │
│   ║  │  └─────────────┘  └─────────────┘  └─────────────┘              │         ║   │
│   ║  └─────────────────────────────────────────────────────────────────┘         ║   │
│   ╚═══════════════════════════════════════════════════════════════════════════════╝   │
│                                    │                                                   │
│   ╔════════════════════════════════╪══════════════════════════════════════════════╗   │
│   ║                      DATA LAYER                                                ║   │
│   ╠════════════════════════════════╪══════════════════════════════════════════════╣   │
│   ║  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐           ║   │
│   ║  │ PostgreSQL  │  │ MongoDB     │  │ Redis       │  │ Blob        │           ║   │
│   ║  │ (Relational)│  │ (Documents) │  │ (Cache)     │  │ Storage     │           ║   │
│   ║  │             │  │             │  │             │  │             │           ║   │
│   ║  │ • Users     │  │ • Features  │  │ • Sessions  │  │ • ML Models │           ║   │
│   ║  │ • Consents  │  │ • Risk Data │  │ • Tokens    │  │ • Exports   │           ║   │
│   ║  │ • Audit     │  │ • Insights  │  │ • Rate Lim. │  │ • Backups   │           ║   │
│   ║  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘           ║   │
│   ║                                                                               ║   │
│   ║  ┌─────────────────────────────────────────────────────────────────┐         ║   │
│   ║  │ Message Queue: Apache Kafka / Azure Event Hubs                   │         ║   │
│   ║  │ • Event Streaming  • Async Processing  • Audit Events           │         ║   │
│   ║  └─────────────────────────────────────────────────────────────────┘         ║   │
│   ╚═══════════════════════════════════════════════════════════════════════════════╝   │
│                                                                                        │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Component Architecture

### 2.1 Mobile Application Architecture

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                         MOBILE APP ARCHITECTURE (MVVM)                             │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                    │
│   ┌────────────────────────────────────────────────────────────────────────────┐  │
│   │                            PRESENTATION LAYER                               │  │
│   │  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐           │  │
│   │  │ Dashboard  │  │ Journal    │  │ Voice      │  │ Settings   │           │  │
│   │  │ View       │  │ View       │  │ Check-in   │  │ View       │           │  │
│   │  │            │  │            │  │ View       │  │            │           │  │
│   │  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘           │  │
│   └────────┼───────────────┼───────────────┼───────────────┼───────────────────┘  │
│            │               │               │               │                       │
│   ┌────────┼───────────────┼───────────────┼───────────────┼───────────────────┐  │
│   │        ▼               ▼               ▼               ▼                    │  │
│   │                          VIEW MODELS                                        │  │
│   │  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐           │  │
│   │  │ Dashboard  │  │ Journal    │  │ Voice      │  │ Settings   │           │  │
│   │  │ ViewModel  │  │ ViewModel  │  │ ViewModel  │  │ ViewModel  │           │  │
│   │  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘           │  │
│   └────────┼───────────────┼───────────────┼───────────────┼───────────────────┘  │
│            │               │               │               │                       │
│   ┌────────┼───────────────┼───────────────┼───────────────┼───────────────────┐  │
│   │        ▼               ▼               ▼               ▼                    │  │
│   │                          DOMAIN LAYER                                       │  │
│   │  ┌────────────────────────────────────────────────────────────────────┐   │  │
│   │  │  Use Cases: GetRiskScore | SaveJournal | ProcessVoice | Sync       │   │  │
│   │  └─────────────────────────────────┬──────────────────────────────────┘   │  │
│   └────────────────────────────────────┼──────────────────────────────────────┘  │
│                                        │                                          │
│   ┌────────────────────────────────────┼──────────────────────────────────────┐  │
│   │                          DATA LAYER                                        │  │
│   │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐            │  │
│   │  │ API Repository  │  │ Local DB        │  │ Secure Storage  │            │  │
│   │  │ (Retrofit/      │  │ (SQLite/        │  │ (Keychain/      │            │  │
│   │  │  Alamofire)     │  │  Room/CoreData) │  │  Keystore)      │            │  │
│   │  └─────────────────┘  └─────────────────┘  └─────────────────┘            │  │
│   └───────────────────────────────────────────────────────────────────────────┘  │
│                                                                                    │
│   ┌────────────────────────────────────────────────────────────────────────────┐  │
│   │                          EDGE AI LAYER                                      │  │
│   │  ┌─────────────────────────────────────────────────────────────────────┐  │  │
│   │  │  On-Device ML: CoreML (iOS) / TensorFlow Lite (Android)             │  │  │
│   │  │                                                                      │  │  │
│   │  │  ┌─────────────────┐     ┌─────────────────┐                        │  │  │
│   │  │  │ Voice Prosody   │     │ Quick NLP       │                        │  │  │
│   │  │  │ Extractor       │     │ (Sentiment)     │                        │  │  │
│   │  │  │ (Privacy First) │     │                 │                        │  │  │
│   │  │  └─────────────────┘     └─────────────────┘                        │  │  │
│   │  └─────────────────────────────────────────────────────────────────────┘  │  │
│   └────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                    │
└──────────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Backend Microservices

| Service | Technology | Responsibility |
|---------|------------|----------------|
| **API Gateway** | Kong / AWS API Gateway | Routing, auth, rate limiting |
| **Auth Service** | Node.js + Express | JWT, OAuth 2.0, MFA |
| **User Service** | Python + FastAPI | Profile management, preferences |
| **Consent Service** | Python + FastAPI | GDPR compliance, consent tracking |
| **Text Ingestion** | Python + FastAPI | Text preprocessing, NLP trigger |
| **Voice Ingestion** | Python + FastAPI | Feature vector storage |
| **Behavioral Ingestion** | Python + FastAPI | Wearable data aggregation |
| **Risk Scoring** | Python + FastAPI | Multi-modal fusion, scoring |
| **Recommendation** | Python + FastAPI | Intervention suggestions |
| **Notification** | Node.js + Express | Push, Email, SMS |
| **Analytics** | Python + FastAPI | Aggregate reporting |

---

## 3. AI/ML Architecture

### 3.1 ML Pipeline Architecture

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                            AI/ML PIPELINE ARCHITECTURE                             │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                    │
│   ┌─────────────────────────────────────────────────────────────────────────────┐ │
│   │                         DATA PIPELINE                                        │ │
│   │  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐                 │ │
│   │  │ Raw Data │──►│ Cleaning │──►│ Feature  │──►│ Feature  │                 │ │
│   │  │ Sources  │   │ & Valid. │   │ Extract  │   │ Store    │                 │ │
│   │  └──────────┘   └──────────┘   └──────────┘   └──────────┘                 │ │
│   └─────────────────────────────────────────────────────────────────────────────┘ │
│                                        │                                          │
│                                        ▼                                          │
│   ┌─────────────────────────────────────────────────────────────────────────────┐ │
│   │                        TRAINING PIPELINE                                     │ │
│   │                                                                              │ │
│   │   ┌──────────────────────────────────────────────────────────────────────┐ │ │
│   │   │  Model Training Jobs (Azure ML / GCP Vertex AI)                      │ │ │
│   │   │                                                                       │ │ │
│   │   │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐            │ │ │
│   │   │  │ NLP Model     │  │ Voice Model   │  │ Anomaly Model │            │ │ │
│   │   │  │ Training      │  │ Training      │  │ Training      │            │ │ │
│   │   │  │               │  │               │  │               │            │ │ │
│   │   │  │ BERT Fine-tune│  │ CNN/RNN       │  │ Isolation     │            │ │ │
│   │   │  │ Classification│  │ Prosody       │  │ Forest        │            │ │ │
│   │   │  └───────────────┘  └───────────────┘  └───────────────┘            │ │ │
│   │   │                              │                                        │ │ │
│   │   │                              ▼                                        │ │ │
│   │   │  ┌────────────────────────────────────────────────────────────────┐ │ │ │
│   │   │  │               VALIDATION & BIAS TESTING                        │ │ │ │
│   │   │  │  • Performance Metrics  • Fairness Metrics  • Sensitivity Tests│ │ │ │
│   │   │  └────────────────────────────────────────────────────────────────┘ │ │ │
│   │   └──────────────────────────────────────────────────────────────────────┘ │ │
│   │                                        │                                   │ │
│   │                                        ▼                                   │ │
│   │   ┌────────────────────────────────────────────────────────────────────┐  │ │
│   │   │                    MODEL REGISTRY (MLflow)                          │  │ │
│   │   │  • Version Control  • Staging/Prod Promotion  • A/B Testing        │  │ │
│   │   └────────────────────────────────────────────────────────────────────┘  │ │
│   └─────────────────────────────────────────────────────────────────────────────┘ │
│                                        │                                          │
│                                        ▼                                          │
│   ┌─────────────────────────────────────────────────────────────────────────────┐ │
│   │                        INFERENCE PIPELINE                                    │ │
│   │                                                                              │ │
│   │   ┌───────────────────────────────────────────────────────────────────────┐│ │
│   │   │                REAL-TIME INFERENCE (Azure ML Endpoints)               ││ │
│   │   │                                                                        ││ │
│   │   │   Request ──► Load Balancer ──► Model Instance Pool ──► Response     ││ │
│   │   │                                                                        ││ │
│   │   │   • Auto-scaling based on demand                                      ││ │
│   │   │   • Latency target: <200ms                                            ││ │
│   │   │   • Fallback to cached scores on failure                              ││ │
│   │   └───────────────────────────────────────────────────────────────────────┘│ │
│   │                                                                              │ │
│   │   ┌───────────────────────────────────────────────────────────────────────┐│ │
│   │   │                    BATCH INFERENCE (Daily Risk Refresh)               ││ │
│   │   │                                                                        ││ │
│   │   │   Scheduled Job ──► Feature Store ──► Batch Scoring ──► Database     ││ │
│   │   │                                                                        ││ │
│   │   │   • Runs nightly for comprehensive risk recalculation                 ││ │
│   │   │   • Updates user risk trends                                          ││ │
│   │   └───────────────────────────────────────────────────────────────────────┘│ │
│   └─────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                    │
└──────────────────────────────────────────────────────────────────────────────────┘
```

### 3.2 NLP Model Architecture

```python
# Conceptual Model Architecture - BERT-based Mental Health Classifier

class MentalHealthNLPModel:
    """
    Architecture:
    - Base: RoBERTa-base (125M parameters)
    - Fine-tuned on mental health corpora
    - Multi-task output heads
    """
    
    # Layers:
    # 1. Input Tokenization (RoBERTa Tokenizer)
    # 2. RoBERTa Encoder (12 layers, 768 hidden dim)
    # 3. Pooling Layer
    # 4. Multi-Task Heads:
    #    - Sentiment Classification (3 classes)
    #    - Stress Level Regression (0-100)
    #    - Cognitive Distortion Detection (multi-label)
    #    - Vocabulary Complexity Score (regression)
```

### 3.3 Voice Prosody Model Architecture

```python
# Conceptual Model Architecture - Voice Prosody Analyzer

class VoiceProsodyModel:
    """
    Architecture:
    - Input: Mel-frequency cepstral coefficients (MFCCs)
    - Encoder: 1D CNN + Bi-LSTM
    - Output: Emotion classification + Stress regression
    """
    
    # Feature Extraction (On-Device):
    # - 40 MFCCs
    # - Jitter/Shimmer measurements
    # - F0 (pitch) statistics
    # - Speaking rate metrics
    
    # Cloud Model:
    # 1. Feature Normalization
    # 2. 1D CNN (3 layers, 64-128-256 filters)
    # 3. Bi-LSTM (2 layers, 128 units)
    # 4. Attention Layer
    # 5. Dense Layers (256 -> 64)
    # 6. Multi-output: Emotion (4 classes), Stress (0-100)
```

---

## 4. Data Architecture

### 4.1 Database Schema Overview

```sql
-- Core User Tables (PostgreSQL)
┌─────────────────────────────────────────────────────────────────────┐
│  users                                                               │
├─────────────────────────────────────────────────────────────────────┤
│  id: UUID (PK)                                                       │
│  email: VARCHAR(255) [encrypted]                                     │
│  phone_hash: VARCHAR(64)                                             │
│  created_at: TIMESTAMP                                               │
│  last_active: TIMESTAMP                                              │
│  status: ENUM (active, suspended, deleted)                           │
│  organization_id: UUID (FK)                                          │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│  consents                                                            │
├─────────────────────────────────────────────────────────────────────┤
│  id: UUID (PK)                                                       │
│  user_id: UUID (FK)                                                  │
│  consent_type: ENUM (text, voice, behavioral, notifications)        │
│  granted: BOOLEAN                                                    │
│  granted_at: TIMESTAMP                                               │
│  revoked_at: TIMESTAMP                                               │
│  version: VARCHAR(20)                                                │
│  ip_address: VARCHAR(45)                                             │
└─────────────────────────────────────────────────────────────────────┘

-- Health Data Tables (MongoDB - for flexibility)
┌─────────────────────────────────────────────────────────────────────┐
│  risk_assessments (Collection)                                       │
├─────────────────────────────────────────────────────────────────────┤
│  {                                                                   │
│    "_id": ObjectId,                                                  │
│    "user_id": UUID,                                                  │
│    "timestamp": ISODate,                                             │
│    "overall_score": Number (0-100),                                  │
│    "confidence": Number (0-1),                                       │
│    "components": {                                                   │
│      "nlp_score": Number,                                            │
│      "voice_score": Number,                                          │
│      "behavioral_score": Number                                      │
│    },                                                                │
│    "risk_level": String,                                             │
│    "trend": String,                                                  │
│    "recommendations": [ObjectId]                                     │
│  }                                                                   │
└─────────────────────────────────────────────────────────────────────┘
```

### 4.2 Data Encryption Strategy

| Data Type | At Rest | In Transit | Key Management |
|-----------|---------|------------|----------------|
| User PII | AES-256 | TLS 1.3 | Azure Key Vault |
| Health Data | AES-256 | TLS 1.3 | Azure Key Vault |
| Voice Features | AES-256 | TLS 1.3 | Azure Key Vault |
| Audit Logs | AES-256 | TLS 1.3 | Azure Key Vault |
| Analytics | Anonymized | TLS 1.3 | N/A |

---

## 5. Security Architecture

### 5.1 Security Layers

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                          SECURITY ARCHITECTURE                                     │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                    │
│   ┌─────────────────────────────────────────────────────────────────────────────┐ │
│   │  LAYER 1: PERIMETER SECURITY                                                │ │
│   │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                   │ │
│   │  │ WAF           │  │ DDoS          │  │ Geo-          │                   │ │
│   │  │ (OWASP Rules) │  │ Protection    │  │ Fencing       │                   │ │
│   │  └───────────────┘  └───────────────┘  └───────────────┘                   │ │
│   └─────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                    │
│   ┌─────────────────────────────────────────────────────────────────────────────┐ │
│   │  LAYER 2: IDENTITY & ACCESS                                                 │ │
│   │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                   │ │
│   │  │ OAuth 2.0 /   │  │ MFA           │  │ RBAC          │                   │ │
│   │  │ OIDC          │  │ (TOTP/SMS)    │  │ (Least Priv.) │                   │ │
│   │  └───────────────┘  └───────────────┘  └───────────────┘                   │ │
│   └─────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                    │
│   ┌─────────────────────────────────────────────────────────────────────────────┐ │
│   │  LAYER 3: DATA SECURITY                                                     │ │
│   │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                   │ │
│   │  │ Encryption    │  │ Tokenization  │  │ Data          │                   │ │
│   │  │ (AES-256)     │  │ (PII)         │  │ Masking       │                   │ │
│   │  └───────────────┘  └───────────────┘  └───────────────┘                   │ │
│   └─────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                    │
│   ┌─────────────────────────────────────────────────────────────────────────────┐ │
│   │  LAYER 4: MONITORING & RESPONSE                                             │ │
│   │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                   │ │
│   │  │ SIEM          │  │ Threat        │  │ Incident      │                   │ │
│   │  │ (Sentinel)    │  │ Detection     │  │ Response      │                   │ │
│   │  └───────────────┘  └───────────────┘  └───────────────┘                   │ │
│   └─────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                    │
└──────────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Infrastructure & Deployment

### 6.1 Cloud Infrastructure (Azure)

| Component | Azure Service | Purpose |
|-----------|---------------|---------|
| Container Orchestration | AKS (Kubernetes) | Microservices hosting |
| API Gateway | Azure API Management | API routing, security |
| Relational DB | Azure PostgreSQL | User data, consents |
| Document DB | Azure Cosmos DB (MongoDB API) | Health data, features |
| Cache | Azure Cache for Redis | Sessions, rate limiting |
| ML Platform | Azure Machine Learning | Model training, inference |
| Storage | Azure Blob Storage | Models, exports, backups |
| Message Queue | Azure Event Hubs | Event streaming |
| Key Management | Azure Key Vault | Encryption keys, secrets |
| Monitoring | Azure Monitor + App Insights | Logging, metrics, traces |
| CDN | Azure CDN | Static asset delivery |

### 6.2 Deployment Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           CI/CD PIPELINE (Azure DevOps)                           │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                   │
│   ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐  │
│   │ Commit │──►│ Build  │──►│ Test   │──►│ Scan   │──►│ Stage  │──►│ Prod   │  │
│   │        │   │        │   │        │   │        │   │ Deploy │   │ Deploy │  │
│   └────────┘   └────────┘   └────────┘   └────────┘   └────────┘   └────────┘  │
│       │            │            │            │            │            │         │
│       │            │            │            │            │            │         │
│       ▼            ▼            ▼            ▼            ▼            ▼         │
│   Git Push     Docker       Unit +        Security    Staging      Prod         │
│   Webhook      Build        Integration   (Snyk,      Environment  Environment  │
│                             Tests         SonarQube)  Smoke Tests  Rolling      │
│                                                                    Update       │
│                                                                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Technology Stack Summary

| Layer | Technology | Justification |
|-------|------------|---------------|
| **Mobile - iOS** | Swift, SwiftUI, CoreML | Native performance, Apple ecosystem |
| **Mobile - Android** | Kotlin, Jetpack Compose, TF Lite | Native performance, Google ecosystem |
| **Web Frontend** | React 18, TypeScript, Tailwind | Modern, maintainable, accessible |
| **API Gateway** | Kong | Open-source, scalable, plugin ecosystem |
| **Backend Services** | Python 3.11, FastAPI | ML ecosystem, async support, speed |
| **Auth Service** | Node.js, Express | JWT handling, OAuth integration |
| **ML Framework** | PyTorch, Hugging Face | State-of-the-art NLP, community |
| **ML Platform** | Azure ML | Enterprise features, HIPAA compliance |
| **Relational DB** | PostgreSQL 15 | Reliability, JSON support, compliance |
| **Document DB** | MongoDB 6.0 | Flexible schema for health data |
| **Cache** | Redis 7 | Speed, pub/sub, rate limiting |
| **Message Queue** | Apache Kafka | Event streaming, durability |
| **Container Runtime** | Docker, Kubernetes | Portability, orchestration |
| **IaC** | Terraform, Helm | Reproducible infrastructure |
| **Monitoring** | Prometheus, Grafana, Azure Monitor | Observability stack |
| **Logging** | ELK Stack (Elasticsearch, Logstash, Kibana) | Centralized logging |

---

*Document Status: Draft - Pending Review*
