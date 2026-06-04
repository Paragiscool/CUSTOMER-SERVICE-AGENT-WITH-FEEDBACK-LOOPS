# Disambiguation Rules Specification

This document details the strategies and decision trees for resolving ambiguous user intents, specifically focusing on the 6 highly confused intent pairs identified during analysis.

## Confidence Threshold Logic

The NLU Pipeline emits a confidence score (0.0 to 1.0) for the top classified intents. The Dialogue Management Engine (DME) uses these thresholds:

- **High Confidence (> 0.85)**: Proceed automatically with the primary intent.
- **Ambiguous (0.60 - 0.85)**: If the margin between the primary and secondary intent is < 0.15, trigger disambiguation rules.
- **Low Confidence (< 0.60)**: Trigger explicit clarification ("I'm not sure I understood. Did you mean X or Y?").
- **Out of Scope (< 0.40)**: Treat as `GEN-003` (Out of Scope) and gracefully fallback.

---

## Decision Trees for Overlapping Intent Pairs

### Pair 1: `TXN-001` (Transaction Status) vs `TXN-002` (Transaction Dispute)
**Context:** Users often say "Check my recent transfer of 5000", which could mean "Did it go through?" or "I didn't authorize this!".
- **Rule 1 (Entity Bias)**: If negative sentiment words ("fraud", "wrong", "unauthorized", "fail") are present, strongly bias toward `TXN-002`.
- **Rule 2 (Status State)**: Query the Core Banking API. If the transaction status is already `FAILED`, it is likely `TXN-001`. If it is `SUCCESS` but user is complaining, it is `TXN-002`.
- **Clarification Prompt**: *"Are you looking to check if the transfer was successful, or do you want to report an issue with this transaction?"*

### Pair 2: `PRD-001` (Product Info) vs `PRD-005` (Investment Advisory)
**Context:** Users ask "What's a good mutual fund?". NexBank can provide information (PRD-001) but CANNOT provide personalized financial advice (PRD-005).
- **Rule 1 (Guardrail Trigger)**: If the user says "should I", "what do you recommend", or "best for me", flag as `PRD-005` (Advisory).
- **Rule 2 (Action)**: If flagged as `PRD-005`, the safety guardrail MUST block generation and output the templated disclaimer. If `PRD-001`, fetch generic brochures from the KB.
- **Clarification Prompt**: *"I can provide you with the list of mutual funds we offer (Information), but I cannot recommend a specific fund for your portfolio (Advice). Would you like to see the list?"*

### Pair 3: `ACC-003` (Contact Update) vs `ACC-004` (Address Update)
**Context:** Users say "I need to update my details."
- **Rule 1 (Entity Detection)**: If the utterance contains an email structure (`@`) or a 10-digit number, map to `ACC-003`. If it contains street indicators ("Street", "Apt", "Pin code"), map to `ACC-004`.
- **Rule 2 (Auth Requirement Check)**: Both require Biometric auth, but `ACC-004` requires a `proof_document`.
- **Clarification Prompt**: *"Would you like to update your phone number/email address, or your permanent mailing address?"*

### Pair 4: `CMP-001` (New Complaint) vs `CMP-003` (Escalate Complaint)
**Context:** Users say "I want to complain about my card."
- **Rule 1 (Context Lookup)**: Check the user's CRM profile for active complaints in the last 14 days related to "card".
- **Rule 2 (Implicit Routing)**: If an active complaint exists matching the topic, assume `CMP-003` and ask: *"I see you have an open complaint (ID: 1234) about your card. Would you like me to escalate this?"*
- **Clarification Prompt**: (If no context matches) *"Are you registering a new complaint, or following up on an existing one?"*

### Pair 5: `SEC-001` (Fraud Report) vs `TXN-002` (Merchant Dispute)
**Context:** Users say "I didn't make this payment of ₹500 to Amazon." (Could be stolen card vs double-charge).
- **Rule 1 (Card Presence)**: If the user indicates they do not have their physical card, default to `SEC-001` (Fraud) immediately.
- **Rule 2 (Merchant History)**: If the user has a history of legitimate transactions with this merchant, lean towards `TXN-002`.
- **Clarification Prompt**: *"Did you authorize a payment to this merchant but there is an error (Dispute), or is this a completely unauthorized transaction by someone else (Fraud)?"*

### Pair 6: `CRD-001` (Card Block) vs `SEC-001` (Fraud Report)
**Context:** Users say "Block my card, someone used it."
- **Rule 1 (Multi-Intent Trigger)**: In this scenario, BOTH intents should be triggered simultaneously.
- **Rule 2 (Execution Order)**: Safety-critical operations take precedence. Execute `CRD-001` immediately to stop bleeding, then automatically transition to `SEC-001` to capture fraud details and initiate a chargeback.
- **Agent Response**: *"I have immediately blocked your card ending in 4567 to secure your account. Now, let's file a fraud report. Can you identify which recent transactions were unauthorized?"*
