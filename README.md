# NexBank Agentic AI — Customer Service Agent with Feedback Loops

A production-grade, self-improving agentic AI customer service system designed for **NexBank**, a fictional neo-bank serving 2.7 million customers across India handling 18,000 daily interactions.

## Project Status

| Phase | Days | Status | Key Deliverables |
| :--- | :--- | :--- | :--- |
| Foundation & Architecture | 1-3 | ✅ Complete | System architecture (9 components), 16-state dialogue machine, interface contracts |
| Intent Taxonomy & NLU | 4-6 | ✅ Complete | 37-intent taxonomy, entity extraction rules, 6 disambiguation trees, 370 utterances |
| Knowledge Base & Retrieval | 7-8 | ✅ Complete | KB schema, 55 entries, hybrid retrieval pipeline |
| Guardrails & Safety | 9-10 | ✅ Complete | Advice guardrails, 60 adversarial test cases |
| Continuous Learning | 11 | ✅ Complete | 5-stage pipeline, CSAT loops, A/B deployment simulator |
| Escalation & Prompts | 12-13 | ✅ Complete | 15 escalation triggers, 20 metrics, 3 dashboards, 6-layer prompt, 16 templates |
| Finalization | 14-15 | 🔲 Pending | Sample conversations, audit spec, risk matrix |

## Repository Structure

```
├── docs/
│   ├── architecture/
│   │   ├── system_architecture.md          # 9-component layered architecture spec
│   │   └── component_interface_contracts.md # JSON API contracts for all services
│   ├── intent-taxonomy/
│   │   ├── intent_taxonomy.md              # 37 hierarchical intent categories
│   │   ├── entity_extraction.md            # 15 entity types with validation rules
│   │   └── disambiguation_rules.md         # 6 overlapping intent pair decision trees
│   ├── knowledge-base/
│   │   ├── kb_schema.md                    # Dual-representation schema (Document + Chunk)
│   │   ├── retrieval_pipeline.md           # Hybrid BM25 + Dense retrieval with RRF
│   │   ├── security_matrix.md              # Access Control Matrix & PII Ingestion Gate
│   │   └── knowledge_maintenance.md        # RBI circular update pipeline & sign-off protocol
│   ├── guardrails/
│   │   ├── financial_advice_boundaries.md  # 3-Layer Safety Guard (info vs. advice)
│   │   ├── adversarial_defenses.md         # Sandwich Architecture for 6 attack vectors
│   │   ├── account_security_guardrails.md  # 8 mandatory account security rules
│   │   ├── regulatory_compliance.md        # RBI, PCI DSS, KYC/AML, DPDP Act mapping
│   │   ├── incident_response_playbook.md   # Breach containment & remediation steps
│   │   └── monitoring_dashboard.md         # False positive analysis & security tracking
│   ├── learning-pipeline/
│   │   ├── learning_architecture.md        # 5-stage closed-loop improvement cycle
│   │   ├── feedback_integration.md         # CSAT, implicit signals & supervisor taxonomy
│   │   └── safety_invariant_protocol.md    # Immutable safety layer & 200-case Air Gap
│   ├── escalation/
│   │   ├── escalation_routing.md           # 15 triggers, decision tree, 8-element handoff
│   │   └── escalation_quality_metrics.md   # Efficiency, appropriateness & load metrics
│   ├── metrics/
│   │   ├── csat_metrics_framework.md       # 20 KPIs (leading, lagging, operational)
│   │   └── dashboard_wireframes.md         # 3 dashboard specs (Real-time, Daily, Weekly)
│   └── system-prompt/
│       ├── prompt_architecture.md          # 6-layer architecture with immutability rules
│       └── prompt_templates.md             # 16 prompt templates with trigger conditions
├── diagrams/
│   └── system_architecture.md              # 6 Mermaid diagrams (architecture, state machine, flows)
├── config/
│   ├── training_utterances.json            # 370 sample utterances across 37 intents
│   └── sample_kb_entries.json              # 55 KB entries (products, policies, regulatory)
├── simulations/
│   └── ab_deployment_simulator.html        # Interactive D3.js A/B testing visualizer
├── tests/
│   └── adversarial_test_cases.json         # 60 adversarial test cases across 8 categories
├── CHANGELOG.md                            # Daily progress log
├── 15_DAY_ROADMAP.md                       # Project methodology & phased roadmap
└── README.md
```

## Architecture Overview

The system is composed of **9 core components** across 4 layers:

| Layer | Components |
| :--- | :--- |
| **Channel & Gateway** | C1: User Interface Gateway |
| **Understanding** | C2: NLU Pipeline, C3: Dialogue Management Engine |
| **Action & Generation** | C4: Knowledge Base, C5: Response Generation, C6: Guardrail Engine, C7: Escalation Router |
| **Platform & Intelligence** | C8: Continuous Learning Pipeline, C9: Audit & Logging Service |

## Key Design Targets

| Metric | Target |
| :--- | :--- |
| End-to-End Latency (p95) | < 3,000ms |
| CSAT Score | 4.5 / 5.0 |
| Containment Rate | 70%+ |
| Incorrect Financial Advice Rate | 0% |
| First Reply Time | < 30 seconds |
| Knowledge Retrieval Latency (p95) | < 200ms |

## Naming Conventions

- **Intent IDs**: `DOMAIN-NNN` format (e.g., `ACC-001`, `TXN-002`, `SEC-001`)
- **Component IDs**: `C1` through `C9`
- **State IDs**: `S0` through `S15`
- **Failure IDs**: `F01` through `F12`
