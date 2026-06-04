# Project Changelog

## Days 4-6 — Intent Taxonomy & NLU Pipeline Design
**Date:** 2026-06-04
**Focus:** Complete intent classification, entity extraction, and disambiguation.

### Work Completed
- Designed **Intent Taxonomy** containing **37 categories** structured across 7 domains: Account Management (7), Transaction & Payment (6), Card Management (6), Product & Advisory (5), Complaint & Feedback (5), Security & Fraud (4), and General Conversational (4).
- Specified required entities, optional entities, authentication level, and safety rating for each of the 37 intents.
- Detailed **Entity Extraction Rules** mapping regex validations, sensitivity labels, and fallback slot-filling logic for 15 core entities (e.g., `account_id`, `card_last_4`, `amount`).
- Established **Disambiguation Decision Trees** for the 6 specific overlapping intent pairs to handle complex context-aware routing correctly.
- Generated a library of **370 Sample Utterances** (10 per intent) for NLU training and stored in `config/training_utterances.json`.

### Design Decisions
1. **37 intents (not 25):** Expanded beyond the minimum 25 with CRD-006 (PIN Reset), GEN-001–004 (conversational bookends), and fine-grained Card Management sub-intents.
2. **Taxonomy ID convention:** All intents use `DOMAIN-NNN` format (e.g., `ACC-001`, `TXN-002`) for unambiguous cross-referencing across architecture, contracts, and training data.
3. **Multi-intent handling (Pair 6):** Card Block + Fraud Report triggers both intents simultaneously — safety-critical action executes first, then fraud intake follows.

### Deliverables Produced
- `docs/intent-taxonomy/intent_taxonomy.md`
- `docs/intent-taxonomy/entity_extraction.md`
- `docs/intent-taxonomy/disambiguation_rules.md`
- `config/training_utterances.json`

### Next Phase Plan
- Phase 2: Knowledge Base & Retrieval Architecture (Days 7-8).

---

## Day 3 — Component Interface Contracts
**Date:** 2026-06-04
**Focus:** Detailed inter-component communication schemas with full JSON payloads.

### Work Completed
- Specified **8 Component Interface Contracts** with full request/response JSON payloads covering all inter-service communication paths.
- Documented payloads for: `C1 Gateway → C3 DME`, `C3 DME → C2 NLU`, `C3 DME → C6 Guardrails (Pre/Post check)`, `C3 DME → C4 KB`, `C3 DME → C5 Response Engine`, `C3 DME → C7 Escalation`, and `All → C9 Audit Logging`.
- Defined exact metadata and payload attributes for cross-component validation.

### Deliverables Produced
- `docs/architecture/component_interface_contracts.md`

### Next Day Plan
- Begin Intent Taxonomy design (Days 4-6).

---

## Day 2 — Architecture Design & System Specification
**Date:** 2026-06-04
**Focus:** High-level system architecture, dialogue state machine, component interfaces, failure modes, latency budget, and scalability specifications.

### Work Completed
- Designed **9-component layered architecture** organised into 4 layers (Channel & Gateway, Understanding, Action & Generation, Platform & Intelligence).
- Created **Dialogue State Machine** with 16 states (S0–S15) and 30+ transition rules with guard conditions covering all conversation flows including authentication, slot-filling, disambiguation, escalation, error recovery, and session timeout.
- Defined **complete dialogue state schema** (JSON) tracking intent classification, entity slots, sentiment trajectory, authentication level, guardrail state, and context flags.
- Created **Failure Mode Analysis** with 12 failure scenarios (F01–F12) covering NLU timeouts, low confidence, state corruption, KB failures, LLM errors, hallucination, guardrail failures, escalation unavailability, and DDoS.
- Designed **cascading failure protection** with bulkhead pattern, circuit breakers, timeout cascades, and safe mode.
- Allocated **latency budget** across 8 pipeline stages totalling <2,500ms (p95) with 500ms buffer, including parallel execution of input guardrails alongside NLU.
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
- Define component interface contracts with full JSON schemas.

---

## Day 1
**Date:** 2026-06-03
- Initialized repository
- Parsed project PDF and extracted methodology
- Generated 15-day roadmap
- Scaffolded mandatory repository directory structure
