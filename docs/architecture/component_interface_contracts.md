# Component Interface Contracts

This document specifies the internal communication protocols and JSON schemas for the core components of the NexBank Agentic AI Customer Service system. All communications occur via gRPC/REST over TLS 1.3.

## 1. Gateway -> Dialogue Management Engine (DME)

The UI Gateway normalises incoming messages and passes them to the DME.

### Request payload
```json
{
  "session_id": "uuid",
  "customer_id": "uuid_or_null",
  "channel": "whatsapp|web|app",
  "auth_level": "anonymous|otp|biometric|kyc",
  "timestamp": "ISO-8601",
  "message": {
    "type": "text|audio|quick_reply",
    "content": "What is my balance?",
    "language": "en-IN"
  }
}
```

## 2. DME -> NLU Pipeline

The DME forwards the sanitised text and current context to the NLU Pipeline for classification and extraction.

### Request Payload
```json
{
  "session_id": "uuid",
  "turn_id": "uuid",
  "raw_text": "What is my balance?",
  "active_intent": "null_or_string",
  "context_entities": {}
}
```

### Response Payload
```json
{
  "primary_intent": {
    "label": "ACC-001",
    "confidence": 0.94
  },
  "secondary_intent": {
    "label": "ACC-002",
    "confidence": 0.42
  },
  "entities": [
    {
      "type": "account_type",
      "value": "savings",
      "confidence": 0.88,
      "validated": true
    }
  ],
  "sentiment": {
    "score": 0.1,
    "emotion": "neutral"
  },
  "out_of_scope": false
}
```

## 3. DME -> Guardrail & Safety Engine (Pre-computation / Input Check)

Before executing policies or DB queries, DME checks inputs for prompt injection or PII violations.

### Request Payload
```json
{
  "session_id": "uuid",
  "turn_id": "uuid",
  "content_to_check": "What is my balance?",
  "check_type": "input"
}
```

### Response Payload
```json
{
  "status": "PASS|FAIL",
  "violations": [],
  "pii_detected": false,
  "sanitised_content": "What is my balance?"
}
```

## 4. DME -> Knowledge Base & Retrieval

When the intent requires factual data, DME queries the KB.

### Request Payload
```json
{
  "query": "savings account balance check",
  "intent": "ACC-001",
  "auth_level": "otp",
  "top_k": 3
}
```

### Response Payload
```json
{
  "results": [
    {
      "doc_id": "kb-049",
      "content": "To check balance, fetch data from Core Banking API endpoint /v1/accounts/{id}/balance.",
      "relevance_score": 0.92,
      "metadata": {
        "category": "internal_procedure",
        "ttl": "3600"
      }
    }
  ],
  "confidence": "HIGH"
}
```

## 5. DME -> Response Generation Engine

DME instructs the RGE to formulate the final response.

### Request Payload
```json
{
  "session_id": "uuid",
  "turn_id": "uuid",
  "intent": "ACC-001",
  "generation_method": "template",
  "template_id": "TMPL-ACC-BAL-001",
  "variables": {
    "account_type": "Savings",
    "account_last_4": "1234",
    "balance": "₹45,230.50"
  },
  "language": "en-IN",
  "tone": "professional"
}
```

### Response Payload
```json
{
  "generated_text": "Your NexBank Savings Account ending in 1234 has a current available balance of ₹45,230.50.",
  "latency_ms": 210,
  "method_used": "template"
}
```

## 6. DME -> Guardrail & Safety Engine (Post-computation / Output Check)

Before sending the response to the user, check for hallucination, unfulfilled promises, or incorrect financial advice.

### Request Payload
```json
{
  "session_id": "uuid",
  "turn_id": "uuid",
  "content_to_check": "Your NexBank Savings Account ending in 1234 has a current available balance of ₹45,230.50.",
  "check_type": "output",
  "grounding_context": "..."
}
```

### Response Payload
```json
{
  "status": "PASS",
  "violations": [],
  "hallucination_detected": false,
  "financial_advice_detected": false
}
```

## 7. DME -> Escalation Router

If criteria are met, the DME triggers a handoff.

### Request Payload
```json
{
  "session_id": "uuid",
  "customer_id": "uuid",
  "trigger_condition": "low_sentiment",
  "priority": "P2",
  "conversation_summary": "Customer frustrated about a missing transaction of ₹5000.",
  "current_intent": "TXN-002"
}
```

### Response Payload
```json
{
  "status": "QUEUED",
  "estimated_wait_time_sec": 120,
  "queue_name": "Senior Resolution Mgr"
}
```

## 8. Asynchronous -> Audit & Logging Service

Components asynchronously fire log events to the logging service.

### Request Payload
```json
{
  "event_type": "turn_completed",
  "session_id": "uuid",
  "turn_id": "uuid",
  "timestamp": "ISO-8601",
  "data": {
    "intent_classified": "ACC-001",
    "guardrails_triggered": [],
    "latency_ms": 2450
  }
}
```
