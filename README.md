<div align="center">
  <h1>🏦 NexBank Agentic AI</h1>
  <h3><i>Tier-1 Enterprise Customer Service Architecture</i></h3>
  <p>A production-grade, self-improving agentic AI blueprint designed for <b>NexBank</b>, a neo-bank serving 2.7M customers across India and handling 18,000 daily interactions.</p>
  <p>
    <b>Strictly compliant with RBI Digital Lending Guidelines, DPDP Act, and PCI DSS.</b>
  </p>
</div>

---

## 🚀 Technical Elevator Pitch

The NexBank Agentic AI is a deterministic, defense-in-depth LLM microservice architecture. It combines a **37-Intent NLU classification pipeline** with a mathematically grounded **Hybrid RAG Fusion engine ($RRF_{k=60}$)**, enabling a sub-2.5 second $P_{95}$ latency. Security is guaranteed via a **"Sandwich" Guardrail Engine**—enforcing strict pre-flight PII masking and post-flight adversarial output scanning—completely decoupling LLM text generation from subjective financial advice liability.

---

## 🏗️ Layered System Overview

```ascii
[ 📱 CHANNELS: Web Chat, App, WhatsApp, IVR ]
                       │
                       ▼
┌───────────────────────────────────────────────┐
│ LAYER 1: GATEWAY & SECURITY                   │
│  [C1] UI Gateway (TLS, Session, DDoS)         │
│  [C6a] Pre-Flight Ingestion (PII Masking)     │
└──────────────────────┬────────────────────────┘
                       ▼
┌───────────────────────────────────────────────┐
│ LAYER 2: UNDERSTANDING & ROUTING              │
│  [C2] NLU Pipeline (37 Intents, Entities)     │
│  [C3] Dialogue State Machine (16 States)      │
└──────────┬───────────────────────┬────────────┘
           │                       │
           ▼                       ▼
┌────────────────────┐   ┌──────────────────────┐
│ LAYER 3: RAG & LLM │   │ LAYER 3: ESCALATION  │
│ [C4] Hybrid KB RRF │   │ [C7] 15 SLA Triggers │
│ [C5] LLM Response  │   │ [C6b] Post-Flight    │
└──────────┬─────────┘   └─────────┬────────────┘
           │                       │
           ▼                       ▼
┌───────────────────────────────────────────────┐
│ LAYER 4: INTELLIGENCE & COMPLIANCE            │
│  [C8] 5-Stage Continuous Learning Pipeline    │
│  [C9] Encrypted WORM Audit Ledger (AES-256)   │
└───────────────────────────────────────────────┘
```

---

## 📂 Repository Directory Tree

This hyperlinked map outlines the complete architectural specification.

```text
├── docs/
│   ├── architecture/
│   │   ├── [system_architecture.md](docs/architecture/system_architecture.md)          # The master 9-component, 4-layer microservice topology spec.
│   │   └── [component_interface_contracts.md](docs/architecture/component_interface_contracts.md) # Exact JSON schemas governing inter-service API calls.
│   ├── intent-taxonomy/
│   │   ├── [intent_taxonomy.md](docs/intent-taxonomy/intent_taxonomy.md)              # Defines 37 hierarchical intent categories across 7 banking domains.
│   │   ├── [entity_extraction.md](docs/intent-taxonomy/entity_extraction.md)            # Regex validation, fallback logic, and sensitivity for 15 entity types.
│   │   └── [disambiguation_rules.md](docs/intent-taxonomy/disambiguation_rules.md)         # Decision trees mapping overlap between 6 ambiguous intent pairs.
│   ├── knowledge-base/
│   │   ├── [kb_schema.md](docs/knowledge-base/kb_schema.md)                    # Dual-representation schema supporting Document & Semantic Chunk layers.
│   │   ├── [retrieval_pipeline.md](docs/knowledge-base/retrieval_pipeline.md)           # Visualizes the Hybrid BM25 + Dense retrieval pipeline using RRF math.
│   │   ├── [security_matrix.md](docs/knowledge-base/security_matrix.md)              # The Access Control Matrix and pre-retrieval PII scrubbing gate.
│   │   └── [knowledge_maintenance.md](docs/knowledge-base/knowledge_maintenance.md)        # Deterministic CI/CD pipeline for updating RBI circulars safely.
│   ├── guardrails/
│   │   ├── [financial_advice_boundaries.md](docs/guardrails/financial_advice_boundaries.md)  # Core logic dividing factual info from prohibited subjective advice.
│   │   ├── [adversarial_defenses.md](docs/guardrails/adversarial_defenses.md)         # Architectural defenses spanning 6 vectors (Jailbreaking, RAG Poisoning).
│   │   ├── [account_security_guardrails.md](docs/guardrails/account_security_guardrails.md)  # 8 non-negotiable step-up MFA and session invalidation rules.
│   │   ├── [regulatory_compliance.md](docs/guardrails/regulatory_compliance.md)        # System mapping to DPDP Act, PCI DSS, and RBI Lending Guidelines.
│   │   ├── [incident_response_playbook.md](docs/guardrails/incident_response_playbook.md)   # Breach containment protocols and human-escalation pathways.
│   │   └── [monitoring_dashboard.md](docs/guardrails/monitoring_dashboard.md)         # Real-time threat telemetry and false positive diagnostic tracking.
│   ├── learning-pipeline/
│   │   ├── [learning_architecture.md](docs/learning-pipeline/learning_architecture.md)        # 5-stage closed-loop, asynchronous human-in-the-loop ML ops cycle.
│   │   ├── [feedback_integration.md](docs/learning-pipeline/feedback_integration.md)         # Implicit behavioral metrics and explicit CSAT signal mapping.
│   │   └── [safety_invariant_protocol.md](docs/learning-pipeline/safety_invariant_protocol.md)    # The 200-case Air Gap suite preventing LoRA updates from eroding safety.
│   ├── escalation/
│   │   ├── [escalation_routing.md](docs/escalation/escalation_routing.md)           # 15 priority triggers, the 8-element payload handoff, and queue management.
│   │   └── [escalation_quality_metrics.md](docs/escalation/escalation_quality_metrics.md)   # Time-to-pickup SLAs, overflow rates, and appropriateness scoring.
│   ├── metrics/
│   │   ├── [csat_metrics_framework.md](docs/metrics/csat_metrics_framework.md)       # 20 Leading, Lagging, and Operational KPIs driving system health.
│   │   └── [dashboard_wireframes.md](docs/metrics/dashboard_wireframes.md)         # Visual layouts for Real-time, Daily Performance, and Weekly Strategy.
│   └── compliance/
│       ├── [audit_logging_spec.md](docs/compliance/audit_logging_spec.md)           # AES-256 schemas enforcing 7-year WORM retention on Turn & Session logs.
│       └── [risk_assessment_matrix.md](docs/compliance/risk_assessment_matrix.md)       # 13 Technical, Business, and Ethical risks paired with architectural mitigations.
├── diagrams/
│   └── [system_architecture.md](diagrams/system_architecture.md)              # Granular Mermaid.js flowcharts detailing topology, RAG, and State.
├── config/
│   ├── [training_utterances.json](config/training_utterances.json)            # 370 raw customer queries labeled across the 37-intent taxonomy.
│   ├── [sample_kb_entries.json](config/sample_kb_entries.json)              # 55 chunked JSON documents representing products and RBI policies.
│   ├── [prompt_architecture.md](config/prompt_architecture.md)          # 6-layer architecture separating mutable dialogue from immutable safety rules.
│   └── [prompt_templates.md](config/prompt_templates.md)             # 16 contextual templates covering disambiguation, deflections, and crises.
├── simulations/
│   ├── [ab_deployment_simulator.html](simulations/ab_deployment_simulator.html)        # Interactive D3.js visualization mapping Canary releases and rollbacks.
│   ├── [design_patterns.md](simulations/design_patterns.md)              # 5 mandatory conversational UX patterns and 8 strictly prohibited anti-patterns.
│   └── [sample_conversations.md](simulations/sample_conversations.md)         # 21 multi-turn transcripts demonstrating guardrails and intent state-switching.
├── tests/
│   └── [adversarial_test_cases.json](tests/adversarial_test_cases.json)         # 60 red-team injection payloads designed to stress test the Sandwich Architecture.
├── [CHANGELOG.md](CHANGELOG.md)                            # The immutable daily development diary of all structural decisions.
└── [15_DAY_ROADMAP.md](15_DAY_ROADMAP.md)                       # The foundational execution plan guiding this repository.
```

---

## 🎯 Key Design Targets

| Metric | Target |
| :--- | :--- |
| End-to-End Latency ($P_{95}$) | < 3,000ms |
| CSAT Score | ≥ 4.5 / 5.0 |
| Containment Rate | 70%+ |
| Incorrect Financial Advice Rate | 0% (Zero Tolerance) |
| Knowledge Retrieval Latency ($P_{95}$) | < 200ms |

## 🏷️ Naming Conventions

- **Intent IDs**: `DOMAIN-NNN` (e.g., `ACC-001`, `TXN-002`, `SEC-001`)
- **Component IDs**: `C1` through `C9`
- **State IDs**: `S0` through `S15`
- **Failure IDs**: `F01` through `F12`
