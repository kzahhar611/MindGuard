# API Specification
## MindGuard - AI Mental Health Early Detection System

---

## 1. API Overview

| Property | Value |
|----------|-------|
| **Base URL** | `https://api.mindguard.io/v1` |
| **Protocol** | HTTPS (TLS 1.3) |
| **Format** | JSON |
| **Authentication** | Bearer JWT Token |
| **Rate Limiting** | 100 requests/minute per user |

---

## 2. Authentication

### 2.1 Login
```
POST /auth/login
```

**Request:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response (200):**
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "refresh_token": "dGhpcyBpcyBhIHJlZnJl...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

### 2.2 Refresh Token
```
POST /auth/refresh
```

### 2.3 Register
```
POST /auth/register
```

**Request:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "organization_code": "CORP123"
}
```

---

## 3. User Endpoints

### 3.1 Get Profile
```
GET /users/me
```

**Response:**
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "created_at": "2024-01-15T10:30:00Z",
  "preferences": {
    "notifications": true,
    "check_in_time": "09:00"
  }
}
```

### 3.2 Update Profile
```
PATCH /users/me
```

---

## 4. Consent Endpoints

### 4.1 Get Consents
```
GET /consents
```

**Response:**
```json
{
  "consents": [
    {
      "type": "text_analysis",
      "granted": true,
      "granted_at": "2024-01-15T10:35:00Z",
      "version": "1.2"
    },
    {
      "type": "voice_analysis",
      "granted": true,
      "granted_at": "2024-01-15T10:35:00Z"
    },
    {
      "type": "behavioral_data",
      "granted": false
    }
  ]
}
```

### 4.2 Update Consent
```
PUT /consents/{type}
```

**Request:**
```json
{
  "granted": true
}
```

---

## 5. Journal Endpoints

### 5.1 List Entries
```
GET /journals?page=1&limit=20
```

### 5.2 Create Entry
```
POST /journals
```

**Request:**
```json
{
  "content": "Today was a productive day...",
  "prompt_id": "uuid-or-null"
}
```

**Response (201):**
```json
{
  "id": "uuid",
  "created_at": "2024-12-09T14:30:00Z",
  "analysis_status": "pending",
  "word_count": 156
}
```

### 5.3 Get Entry
```
GET /journals/{id}
```

### 5.4 Get Entry Analysis
```
GET /journals/{id}/analysis
```

**Response:**
```json
{
  "journal_id": "uuid",
  "sentiment": "positive",
  "sentiment_score": 0.75,
  "cognitive_distortions": [],
  "stress_indicators": 35,
  "analyzed_at": "2024-12-09T14:32:00Z"
}
```

---

## 6. Voice Endpoints

### 6.1 Submit Voice Features
```
POST /voice/features
```

**Request:**
```json
{
  "feature_vector": [0.12, -0.45, 0.87, ...],
  "duration_seconds": 45,
  "device_model": "iPhone 15"
}
```

### 6.2 Get Voice Analysis
```
GET /voice/{id}/analysis
```

---

## 7. Risk Endpoints

### 7.1 Get Current Risk Score
```
GET /risk/current
```

**Response:**
```json
{
  "score": 42,
  "level": "moderate",
  "trend": "stable",
  "confidence": 0.85,
  "components": {
    "text": 38,
    "voice": 45,
    "behavioral": 48
  },
  "updated_at": "2024-12-09T12:00:00Z"
}
```

### 7.2 Get Risk History
```
GET /risk/history?days=30
```

### 7.3 Get Risk Breakdown
```
GET /risk/breakdown
```

---

## 8. Recommendations Endpoints

### 8.1 Get Recommendations
```
GET /recommendations
```

**Response:**
```json
{
  "recommendations": [
    {
      "id": "uuid",
      "type": "exercise",
      "title": "5-Minute Box Breathing",
      "description": "Reduce anxiety with this simple technique",
      "duration_minutes": 5,
      "priority": 1
    }
  ]
}
```

### 8.2 Mark Recommendation Completed
```
POST /recommendations/{id}/complete
```

---

## 9. Resources Endpoints

### 9.1 List Resources
```
GET /resources?category=breathing
```

### 9.2 Get Resource
```
GET /resources/{id}
```

---

## 10. Privacy Endpoints

### 10.1 Request Data Export
```
POST /privacy/export
```

**Response:**
```json
{
  "request_id": "uuid",
  "status": "processing",
  "estimated_completion": "2024-12-10T12:00:00Z"
}
```

### 10.2 Delete Account
```
DELETE /users/me
```

---

## 11. Error Responses

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email format is invalid",
    "details": {
      "field": "email"
    }
  }
}
```

| Status | Code | Description |
|--------|------|-------------|
| 400 | VALIDATION_ERROR | Invalid request |
| 401 | UNAUTHORIZED | Invalid/expired token |
| 403 | FORBIDDEN | Insufficient permissions |
| 404 | NOT_FOUND | Resource not found |
| 429 | RATE_LIMITED | Too many requests |
| 500 | INTERNAL_ERROR | Server error |

---

*Document Status: Draft*
