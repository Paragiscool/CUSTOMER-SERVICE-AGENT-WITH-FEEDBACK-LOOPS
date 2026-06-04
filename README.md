# NexBank Agentic AI — Customer Service Agent with Feedback Loops

A production-grade, self-improving agentic AI customer service system designed for **NexBank**, a fictional neo-bank serving 2.7 million customers across India handling 18,000 daily interactions.

## Project Status

| Phase | Days | Status | Key Deliverables |
| :--- | :--- | :--- | :--- |
| Foundation & Architecture | 1-3 | ✅ Complete | System architecture (9 components), 16-state dialogue machine, interface contracts |
| Intent Taxonomy & NLU | 4-6 | ✅ Complete | 37-intent taxonomy, entity extraction rules, 6 disambiguation trees, 370 utterances |
| Knowledge Base & Retrieval | 7-8 | 🔲 Pending | KB schema, 50+ entries, retrieval pipeline |
| Guardrails & Safety | 9-10 | 🔲 Pending | Financial advice guardrails, adversarial test cases |
| Continuous Learning | 11 | 🔲 Pending | Feedback loops, A/B testing framework |
| Escalation & Prompts | 12-13 | 🔲 Pending | Routing logic, CSAT framework, system prompts |
| Finalization | 14-15 | 🔲 Pending | Sample conversations, audit spec, risk matrix |

## Repository Structure

```
├── docs/
│   ├── architecture/
│   │   ├── system_architecture.md      # 9-component layered architecture spec
│   │   └── component_interface_contracts.md  # JSON API contracts for all services
│   ├── intent-taxonomy/
│   │   ├── intent_taxonomy.md          # 37 hierarchical intent categories
│   │   ├── entity_extraction.md        # 15 entity types with validation rules
│   │   └── disambiguation_rules.md     # 6 overlapping intent pair decision trees
│   ├── guardrails/                     # (Days 9-10)
│   ├── knowledge-base/                 # (Days 7-8)
│   ├── learning-pipeline/              # (Day 11)
│   ├── escalation/                     # (Days 12-13)
│   └── metrics/                        # (Days 12-13)
├── diagrams/
│   └── system_architecture.md          # 6 Mermaid diagrams (architecture, state machine, flows)
├── config/
│   └── training_utterances.json        # 370 sample utterances across 37 intents
├── simulations/                        # (Days 14-15) Sample conversation flows
├── tests/                              # (Days 9-10) Guardrail test cases
├── CHANGELOG.md                        # Daily progress log
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
