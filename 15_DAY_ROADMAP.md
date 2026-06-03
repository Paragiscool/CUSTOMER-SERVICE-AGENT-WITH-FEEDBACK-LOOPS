# 15-Day Project Methodology & Roadmap

Based on the official requirements (PART D: Project Methodology and Deliverables), here is your day-to-day guide for designing and delivering the NexBank Agentic AI Customer Service system.

---

## 📍 Phase 1: Foundation & NLU
### Day 1-3: Project Analysis & Architecture Design
**Focus:** Scenario immersion, stakeholder analysis, and core architecture design.
* **Mandatory Tasks:**
  * Read the entire project document and map dependencies for the 7 core deliverables.
  * Design a high-level system architecture with 8+ components.
  * Create a dialogue state machine specification (states, transitions, guards).
  * Define component interface contracts (input/output).
  * Create failure mode analysis and fallback strategies.
  * Define a latency budget (<3s total end-to-end response time).
  * Begin scalability specifications for 1x, 10x, and 100x load handling.
* **Deliverables Due:**
  * Initial `CHANGELOG.md` entry with analysis notes.
  * Draft architecture document with system diagram.
  * Component interface specifications.

### Day 4-6: Intent Taxonomy & NLU Pipeline Design
**Focus:** Complete intent classification, entity extraction, and disambiguation.
* **Mandatory Tasks:**
  * Design 25+ intent categories with a hierarchical structure (primary > secondary > tertiary).
  * Define entity types with extraction rules and validation constraints.
  * Specify slot-filling rules (required vs. optional, defaults, confirmation strategies).
  * Create disambiguation rules for all 6 specified overlapping intent pairs.
  * Design out-of-scope handling and multi-intent handling.
  * Specify NLU confidence thresholds with layer-specific fallback strategies.
  * Write at least 10 sample utterances per intent category (250+ total).
* **Deliverables Due:**
  * Complete Intent Taxonomy Document.
  * Entity Extraction Specifications.
  * Disambiguation decision trees.
  * Sample utterance library.

---

## 📍 Phase 2: Knowledge & Safety
### Day 7-8: Knowledge Base & Retrieval Architecture
**Focus:** KB schema, retrieval pipeline, and regulatory knowledge management.
* **Mandatory Tasks:**
  * Design the knowledge base schema with an entity-relationship model.
  * Create 50+ sample knowledge base entries across all categories.
  * Specify the retrieval pipeline (dense, sparse, or hybrid fusion).
  * Design cross-encoder re-ranking and contextual retrieval query modification.
  * Specify retrieval confidence scoring with uncertainty thresholds.
  * Design knowledge update workflows and regulatory knowledge management for RBI circulars.
  * Define an access control matrix by authentication level.
* **Deliverables Due:**
  * Knowledge Base Schema Document.
  * 50+ sample KB entries.
  * Retrieval pipeline specification.
  * Knowledge maintenance procedures.

### Day 9-10: Guardrail & Safety Specification
**Focus:** Financial advice guardrails, security boundaries, and adversarial robustness.
* **Mandatory Tasks:**
  * Specify complete financial advice guardrails (permissible vs. prohibited).
  * Define all 8 mandatory account security guardrails.
  * Map regulatory compliance guardrails (RBI, PCI DSS, KYC/AML).
  * Specify defenses for all 6 adversarial attack vectors.
  * Create 50+ adversarial test cases across all guardrail categories.
  * Design guardrail implementations (rule-based, model-based, hybrid).
  * Perform false positive analysis and design a monitoring dashboard.
  * Create an incident response playbook for breaches.
* **Deliverables Due:**
  * Complete Guardrail Specification.
  * 50+ adversarial test cases.
  * Guardrail monitoring dashboard spec.
  * Incident response playbook.

---

## 📍 Phase 3: Continuous Learning & Operations
### Day 11: Continuous Learning Pipeline & A/B Testing
**Focus:** Feedback integration, model update strategy, and safety preservation.
* **Mandatory Tasks:**
  * Design the supervisor correction loop (sampling, interfaces, taxonomy).
  * Design customer satisfaction (CSAT) signal collection.
  * Design resolution outcome tracking and repeat contact monitoring.
  * Specify the data pipeline (collection, preprocessing, privacy gates).
  * Define the model update strategy (frequency, incremental vs. full retraining).
  * Design a safety-preserving update protocol (canary testing, rollback).
  * Specify an A/B testing framework and experiment governance.
  * Specify an immutable safety layer resistant to learning updates.
* **Deliverables Due:**
  * Complete Learning Pipeline Specification.
  * Feedback source integration specs.
  * A/B testing framework.
  * Safety invariant testing protocol.

### Day 12-13: Escalation, Satisfaction & System Prompt Design
**Focus:** Routing logic, metrics framework, and prompt architecture.
* **Mandatory Tasks:**
  * Finalize 15 escalation trigger conditions with priority classification and SLAs.
  * Design routing decision logic and an 8-element handoff protocol.
  * Design queue management (overflow, after-hours, priorities).
  * Define escalation quality metrics and de-escalation logic.
  * Design a complete metrics framework (leading, lagging, operational indicators).
  * Specify the closed-loop improvement cycle (Detect, Diagnose, Design, Deploy, Validate, Document).
  * Design 3 dashboards (Real-time Operations, Daily Performance, Weekly Strategic).
  * Design a layered system prompt architecture (6 layers, immutability rules).
  * Create 15+ prompt templates with trigger conditions.
* **Deliverables Due:**
  * Escalation routing logic document.
  * CSAT framework with dashboard wireframes.
  * System prompt specification and prompt template library.

---

## 📍 Phase 4: Finalization & Submission
### Day 14-15: Integration, Sample Conversations & Final Submission
**Focus:** System coherence, conversation design, documentation polish, and submission.
* **Mandatory Tasks:**
  * Design 20+ sample conversation flows demonstrating end-to-end agent behavior.
  * Verify conversations show dialogue turns, intents, entities, guardrails, and decision points.
  * Apply all 5 mandatory conversation design patterns (e.g., Empathy-First, Progressive Disclosure).
  * Verify absence of all 8 critical anti-patterns.
  * Complete Audit & Compliance Logging Specification.
  * Complete the Risk Assessment Matrix (technical, business, ethical).
  * Polish documentation and cross-reference validation.
  * Finalize `CHANGELOG.md` with all daily entries.
  * Validate against the 10 automatic failure conditions.
  * Transfer the repository to `@ZethetaIntern`.
* **Deliverables Due:**
  * 20+ sample conversations.
  * Audit logging specification.
  * Risk assessment matrix.
  * Complete `CHANGELOG.md`.
  * Mandatory Repository Structure implemented and transferred.

> [!IMPORTANT]
> **Daily Routine Reminder:** Your `CHANGELOG.md` must be updated daily with your work sessions, tasks completed, design decisions, challenges, open questions, and the plan for the next day. This is heavily evaluated!
