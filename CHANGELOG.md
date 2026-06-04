# Project Changelog

## Day 2 — Architecture Design & System Specification
**Date:** 2026-06-04  
**Focus:** High-level system architecture, dialogue state machine, component interfaces, failure modes, latency budget, and scalability specifications.

### Work Completed
- Designed **9-component layered architecture** organised into 4 layers (Channel & Gateway, Understanding, Action & Generation, Platform & Intelligence).
- Created **Dialogue State Machine** with 16 states (S0–S15) and 30+ transition rules with guard conditions covering all conversation flows including authentication, slot-filling, disambiguation, escalation, error recovery, and session timeout.
- Defined **complete dialogue state schema** (JSON) tracking intent classification, entity slots, sentiment trajectory, authentication level, guardrail state, and context flags.
- Specified **6 component interface contracts** with full request/response JSON schemas and SLA targets:
  - C1→C2: Gateway to NLU (p95 < 500ms)
  - C3→C4: Dialogue Engine to KB (p95 < 200ms)
  - C3→C5: Dialogue Engine to Response Gen (p95 < 1500ms)
  - C6: Guardrail validation (p95 < 50ms)
  - C3→C7: Escalation handoff (initiation < 2s)
  - C9: Audit logging (async, fire-and-forget)
- Created **Failure Mode Analysis** with 12 failure scenarios (F01–F12) covering NLU timeouts, low confidence, state corruption, KB failures, LLM errors, hallucination, guardrail failures, escalation unavailability, and DDoS.
- Designed **cascading failure protection** with bulkhead pattern, circuit breakers, timeout cascades, and safe mode.
- Allocated **latency budget** across 10 pipeline stages totalling <3,000ms (p95) with optimisation strategies (parallel execution, speculative retrieval, response caching).
- Created **scalability specifications** for 1x (18K/day), 10x (180K/day), and 100x (1.8M/day) load profiles with per-component scaling strategies and auto-scaling policies.
- Produced **6 Mermaid diagrams**: High-level architecture, request processing sequence, dialogue state machine, failure/escalation flow, latency budget Gantt, and 100x scalability architecture.

### Design Decisions
1. **9 components (not 8):** Separated Escalation Router (C7) from Dialogue Engine (C3) for independent scaling and clearer SLA ownership.
2. **Hybrid retrieval (Dense + BM25 + RRF):** Chosen over pure dense retrieval for better precision on structured banking queries.
3. **Template-first for safety-critical responses:** Financial data always uses immutable templates; LLM only for conversational elements.
4. **Immutable safety layer:** Guardrail Engine (C6) contains a rule-based layer that the Continuous Learning Pipeline (C8) can NEVER modify.
5. **Cloud-agnostic design:** All technology choices have equivalents across AWS, Azure, and GCP.

### Challenges
- Balancing latency budget for LLM-based response generation (1,500ms) while keeping total under 3s — mitigated by parallel execution and speculative retrieval.
- Defining guard conditions for state transitions to avoid deadlocks — resolved with maximum retry counts and fallback-to-escalation pattern.

### Deliverables Produced
- `docs/architecture/system_architecture.md` — Full architecture specification
- `diagrams/system_architecture.md` — 6 Mermaid diagrams

### Next Day Plan
- Begin Intent Taxonomy design (25+ categories with hierarchical structure).
- Define entity types, extraction rules, and validation constraints.
- Create disambiguation rules for the 6 specified overlapping intent pairs.

---

## Days 4-6 — Intent Taxonomy & NLU Pipeline Design
**Date:** 2026-06-04
**Focus:** Complete intent classification, entity extraction, and disambiguation.

### Work Completed
- Designed **Intent Taxonomy** containing 32 categories structured across Account Management, Transaction & Payment, Card Management, Product & Advisory, Complaint & Feedback, Security & Fraud, and General Conversational intents.
- Specified required entities, optional entities, authentication level, and safety rating for each of the 32 intents.
- Detailed **Entity Extraction Rules** mapping regex validations, sensitivity labels, and fallback slot-filling logic for 15 core entities (e.g., account_id, card_last_4, amount).
- Established **Disambiguation Decision Trees** for the 6 specific overlapping intent pairs to handle complex context-aware routing correctly.
- Generated a library of **320 Sample Utterances** (10 per intent) for NLU training and stored in `config/training_utterances.json`.

### Deliverables Produced
- `docs/intent-taxonomy/intent_taxonomy.md`
- `docs/intent-taxonomy/entity_extraction.md`
- `docs/intent-taxonomy/disambiguation_rules.md`
- `config/training_utterances.json`

### Next Phase Plan
- Phase 3: Knowledge Base & Guardrails (Days 7-10).

---

## Day 3 — Component Interfaces & Validation
**Date:** 2026-06-04
**Focus:** Detailed inter-component communication schemas.

### Work Completed
- Specified **Component Interface Contracts** with full request/response JSON payloads for all 9 components.
- Documented payloads for `Gateway -> DME`, `DME -> NLU`, `DME -> Guardrails (Pre/Post check)`, `DME -> KB`, `DME -> Response Engine`, `DME -> Escalation`, and `Async -> Audit Logging`.
- Defined exact metadata and payload attributes for cross-component validation.

### Deliverables Produced
- `docs/architecture/component_interface_contracts.md`

### Next Day Plan
- Begin Phase 2 / Intent Taxonomy design (Days 4-6).

---

## Day 1
**Date:** 2026-06-03
- Initialized repository
- Parsed project PDF and extracted methodology
- Generated 15-day roadmap
- Scaffolded mandatory repository directory structure
