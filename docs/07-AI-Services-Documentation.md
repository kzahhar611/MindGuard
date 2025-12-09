# AI Services & Models Documentation
## MindGuard - AI Mental Health Early Detection System

---

## 1. AI Services Overview

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         AI SERVICES ARCHITECTURE                                  │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                   │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                           INPUT LAYER                                    │   │
│   │   ┌───────────────┐  ┌───────────────┐  ┌───────────────┐               │   │
│   │   │ Text Data     │  │ Voice Data    │  │ Behavioral    │               │   │
│   │   │ (Journals,    │  │ (Prosodic     │  │ Data (Sleep,  │               │   │
│   │   │  Messages)    │  │  Features)    │  │  Activity)    │               │   │
│   │   └───────┬───────┘  └───────┬───────┘  └───────┬───────┘               │   │
│   └───────────┼──────────────────┼──────────────────┼───────────────────────┘   │
│               │                  │                  │                            │
│               ▼                  ▼                  ▼                            │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                        PROCESSING LAYER                                  │   │
│   │   ┌───────────────┐  ┌───────────────┐  ┌───────────────┐               │   │
│   │   │ NLP Service   │  │ Voice         │  │ Anomaly       │               │   │
│   │   │               │  │ Analysis      │  │ Detection     │               │   │
│   │   │ • Sentiment   │  │ Service       │  │ Service       │               │   │
│   │   │ • Distortions │  │               │  │               │               │   │
│   │   │ • Complexity  │  │ • Prosody     │  │ • Baseline    │               │   │
│   │   │               │  │ • Emotion     │  │ • Deviations  │               │   │
│   │   └───────┬───────┘  └───────┬───────┘  └───────┬───────┘               │   │
│   └───────────┼──────────────────┼──────────────────┼───────────────────────┘   │
│               │                  │                  │                            │
│               └──────────────────┼──────────────────┘                            │
│                                  ▼                                               │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                         FUSION LAYER                                     │   │
│   │              ┌────────────────────────────────────┐                     │   │
│   │              │    Multi-Modal Risk Fusion Model   │                     │   │
│   │              │    • Weighted Combination          │                     │   │
│   │              │    • Temporal Context              │                     │   │
│   │              │    • Personal Calibration          │                     │   │
│   │              └────────────────┬───────────────────┘                     │   │
│   └───────────────────────────────┼─────────────────────────────────────────┘   │
│                                   ▼                                              │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                         OUTPUT LAYER                                     │   │
│   │   ┌───────────────┐  ┌───────────────┐  ┌───────────────┐               │   │
│   │   │ Risk Score    │  │ Trend         │  │ Recommendation│               │   │
│   │   │ (0-100)       │  │ Analysis      │  │ Engine        │               │   │
│   │   └───────────────┘  └───────────────┘  └───────────────┘               │   │
│   └─────────────────────────────────────────────────────────────────────────┘   │
│                                                                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. NLP Service

### 2.1 Model Specifications

| Component | Specification |
|-----------|---------------|
| **Base Model** | RoBERTa-base (125M parameters) |
| **Fine-tuning Data** | Mental health text corpus (~500K samples) |
| **Framework** | PyTorch + Hugging Face Transformers |
| **Inference Platform** | Azure ML Managed Endpoints |
| **Latency Target** | < 200ms (95th percentile) |

### 2.2 Model Outputs

```python
# NLP Service Response Schema
{
    "text_id": "uuid",
    "timestamp": "ISO-8601",
    "analysis": {
        "sentiment": {
            "label": "positive|neutral|negative",
            "score": 0.85,  # confidence
            "valence": 0.6  # -1 to 1
        },
        "cognitive_distortions": {
            "all_or_nothing": {"detected": true, "examples": ["always", "never"], "count": 3},
            "catastrophizing": {"detected": false, "examples": [], "count": 0},
            "overgeneralization": {"detected": true, "examples": ["everyone"], "count": 1}
        },
        "vocabulary_metrics": {
            "complexity_score": 65,  # 0-100
            "word_count": 156,
            "avg_word_length": 4.8,
            "unique_word_ratio": 0.72
        },
        "stress_indicators": {
            "score": 45,  # 0-100
            "markers": ["deadline", "overwhelmed"]
        }
    },
    "risk_contribution": 42  # 0-100
}
```

### 2.3 Cognitive Distortion Categories

| Distortion | Keywords/Patterns | Weight |
|------------|-------------------|--------|
| All-or-Nothing | always, never, completely, totally, nothing | 1.0 |
| Catastrophizing | disaster, terrible, awful, worst, ruined | 1.2 |
| Overgeneralization | everyone, no one, all, every time | 0.8 |
| Mind Reading | they think, they know, they hate | 0.7 |
| Should Statements | should, must, ought to, have to | 0.6 |

---

## 3. Voice Analysis Service

### 3.1 Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    VOICE ANALYSIS PIPELINE                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                               │
│   ┌─────────────────────────────────────────────────────────────────────┐   │
│   │                     ON-DEVICE (Edge Processing)                      │   │
│   │                                                                       │   │
│   │   Raw Audio ──► Feature Extraction ──► Numerical Vector              │   │
│   │                 (CoreML/TF Lite)       (40 MFCCs + prosody)          │   │
│   │                                                                       │   │
│   │   🔒 Raw audio deleted immediately after extraction                  │   │
│   └───────────────────────────────┬─────────────────────────────────────┘   │
│                                   │                                          │
│                                   ▼ Encrypted vector only                    │
│                                                                               │
│   ┌─────────────────────────────────────────────────────────────────────┐   │
│   │                     CLOUD (Azure ML)                                 │   │
│   │                                                                       │   │
│   │   Feature Vector ──► Prosody Model ──► Emotion Classification        │   │
│   │                      (CNN + LSTM)       + Stress Regression          │   │
│   └─────────────────────────────────────────────────────────────────────┘   │
│                                                                               │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Prosodic Features

| Feature | Description | Extraction Method |
|---------|-------------|-------------------|
| **MFCC (1-40)** | Mel-frequency cepstral coefficients | Librosa |
| **F0 (Pitch)** | Fundamental frequency mean, std, range | PRAAT/PyAudio |
| **Jitter** | Cycle-to-cycle frequency variation | OpenSMILE |
| **Shimmer** | Cycle-to-cycle amplitude variation | OpenSMILE |
| **HNR** | Harmonics-to-noise ratio | PRAAT |
| **Speaking Rate** | Syllables per second estimate | Custom |
| **Pause Duration** | Mean and std of pauses | VAD detection |
| **Voice Breaks** | Phonation interruptions | Custom |

### 3.3 Model Output

```python
# Voice Analysis Response Schema
{
    "voice_id": "uuid",
    "timestamp": "ISO-8601",
    "duration_seconds": 45,
    "analysis": {
        "emotion": {
            "primary": "neutral|stressed|sad|anxious|calm",
            "confidence": 0.78,
            "secondary": "tired"
        },
        "prosody_indicators": {
            "pace": {"value": "normal", "deviation_from_baseline": 0.1},
            "pitch_variability": {"value": "reduced", "score": 35},
            "pauses": {"frequency": "elevated", "avg_duration_ms": 450}
        },
        "stress_score": 52  # 0-100
    },
    "risk_contribution": 52  # 0-100
}
```

---

## 4. Behavioral Anomaly Detection Service

### 4.1 Data Sources

| Source | Metrics | API |
|--------|---------|-----|
| Apple HealthKit | Sleep, steps, heart rate, workouts | HealthKit |
| Google Fit | Sleep, steps, activities | REST API |
| Device Usage | Screen time, app categories | iOS/Android APIs |

### 4.2 Anomaly Detection Model

| Component | Specification |
|-----------|---------------|
| **Model Type** | Isolation Forest + LSTM Autoencoder |
| **Baseline Period** | 14 days minimum |
| **Update Frequency** | Daily |
| **Anomaly Threshold** | 2 standard deviations |

### 4.3 Behavioral Indicators

| Indicator | Normal Range | Warning | Critical |
|-----------|--------------|---------|----------|
| Sleep Duration | 6-9 hours | <5 or >10 for 3d | <4 or >12 for 5d |
| Sleep Timing | ±1 hour of baseline | ±2 hours for 3d | ±3 hours for 5d |
| Daily Steps | ±30% of baseline | -50% for 3d | -70% for 5d |
| Screen Time | ±20% of baseline | +50% for 3d | +80% for 5d |

### 4.4 Model Output

```python
# Behavioral Analysis Response Schema
{
    "user_id": "uuid",
    "analysis_date": "YYYY-MM-DD",
    "analysis": {
        "sleep": {
            "avg_duration_hours": 5.2,
            "baseline_duration": 7.5,
            "deviation_percentage": -30.6,
            "anomaly_detected": true,
            "days_anomalous": 4
        },
        "activity": {
            "avg_steps": 2100,
            "baseline_steps": 6000,
            "deviation_percentage": -65,
            "anomaly_detected": true,
            "days_anomalous": 5
        },
        "screen_time": {
            "avg_hours": 8.5,
            "baseline_hours": 5.0,
            "deviation_percentage": 70,
            "anomaly_detected": true
        }
    },
    "risk_contribution": 68  # 0-100
}
```

---

## 5. Risk Fusion Model

### 5.1 Multi-Modal Fusion

```python
# Risk Score Calculation
def calculate_risk_score(nlp_score, voice_score, behavioral_score, weights, calibration):
    """
    Weighted multi-modal fusion with personal calibration
    """
    # Default weights (configurable)
    w_nlp = weights.get('nlp', 0.4)
    w_voice = weights.get('voice', 0.3)
    w_behavioral = weights.get('behavioral', 0.3)
    
    # Handle missing data (reduce weight if not available)
    active_weights = normalize_weights(w_nlp, w_voice, w_behavioral, 
                                        has_nlp=nlp_score is not None,
                                        has_voice=voice_score is not None,
                                        has_behavioral=behavioral_score is not None)
    
    # Weighted combination
    raw_score = (
        (nlp_score or 0) * active_weights['nlp'] +
        (voice_score or 0) * active_weights['voice'] +
        (behavioral_score or 0) * active_weights['behavioral']
    )
    
    # Personal calibration adjustment
    calibrated_score = apply_calibration(raw_score, calibration)
    
    return min(100, max(0, calibrated_score))
```

### 5.2 Risk Level Classification

| Score Range | Level | Description | Actions |
|-------------|-------|-------------|---------|
| 0-25 | 🟢 Low | Healthy baseline | Maintenance tips |
| 26-50 | 🟡 Moderate | Minor concerns | Self-help resources |
| 51-75 | 🟠 Elevated | Attention needed | Coaching recommended |
| 76-100 | 🔴 High | Urgent attention | Clinical referral |

---

## 6. Recommendation Engine

### 6.1 Recommendation Categories

| Category | Risk Level | Examples |
|----------|------------|----------|
| **Wellness Tips** | 🟢 Low | Hydration reminders, gratitude prompts |
| **Self-Help** | 🟡 Moderate | Breathing exercises, journaling prompts |
| **Guided Programs** | 🟠 Elevated | 7-day stress reduction, sleep hygiene |
| **Professional** | 🔴 High | Coaching session, therapist referral |
| **Crisis** | 🔴 Critical | Crisis hotline, emergency services |

### 6.2 Recommendation Algorithm

```python
def get_recommendations(risk_score, risk_breakdown, user_preferences, history):
    """
    Context-aware recommendation engine
    """
    recommendations = []
    
    # Risk-based recommendations
    if risk_score < 25:
        recommendations.append(get_maintenance_tip())
    elif risk_score < 50:
        recommendations.append(get_self_help_exercise(risk_breakdown))
    elif risk_score < 75:
        recommendations.append(get_coaching_suggestion())
        recommendations.append(get_intensive_program(risk_breakdown))
    else:
        recommendations.append(get_clinical_referral())
        recommendations.append(get_crisis_resources())
    
    # Personalization based on engagement history
    recommendations = filter_by_preferences(recommendations, user_preferences)
    recommendations = avoid_recent_duplicates(recommendations, history)
    
    return recommendations[:3]  # Top 3 recommendations
```

---

## 7. Model Training & Deployment

### 7.1 ML Pipeline

| Stage | Tool | Frequency |
|-------|------|-----------|
| Data Ingestion | Azure Data Factory | Daily |
| Feature Engineering | Spark on Databricks | Daily |
| Model Training | Azure ML | Weekly |
| Model Validation | Azure ML Pipelines | Weekly |
| Bias Testing | Fairlearn | Weekly |
| Deployment | Azure ML Endpoints | On approval |
| Monitoring | Azure Monitor | Continuous |

### 7.2 Model Versioning

| Version | Type | Description |
|---------|------|-------------|
| `nlp-v1.0.0` | Production | Initial RoBERTa model |
| `nlp-v1.1.0` | Staging | Improved distortion detection |
| `voice-v1.0.0` | Production | Initial prosody model |
| `anomaly-v1.0.0` | Production | Initial behavioral model |

### 7.3 Performance Monitoring

| Metric | NLP Model | Voice Model | Behavioral |
|--------|-----------|-------------|------------|
| Accuracy | >85% | >80% | N/A (unsupervised) |
| F1 Score | >0.82 | >0.78 | N/A |
| Latency (p95) | <200ms | <150ms | <100ms |
| Drift Threshold | 5% | 5% | 10% |

---

## 8. Bias & Fairness

### 8.1 Fairness Metrics

| Metric | Definition | Target |
|--------|------------|--------|
| Demographic Parity | Equal positive rates across groups | <5% gap |
| Equalized Odds | Equal TPR and FPR across groups | <5% gap |
| Calibration | Predicted probabilities match outcomes | Within 10% |

### 8.2 Protected Attributes

- Age groups (18-25, 26-40, 41-60, 60+)
- Gender identity
- Geographic region
- Language/dialect (future)

### 8.3 Bias Mitigation

1. **Pre-processing**: Balanced training data sampling
2. **In-processing**: Fairness constraints in loss function
3. **Post-processing**: Threshold adjustment per group
4. **Monitoring**: Weekly fairness dashboards

---

*Document Status: Draft*
