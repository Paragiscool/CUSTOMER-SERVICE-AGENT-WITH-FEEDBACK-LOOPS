# System Prompt Architecture (6 Layers)

The NexBank Agentic AI uses a layered system prompt architecture. Each layer serves a distinct purpose, has a defined owner, and strict immutability rules that prevent the Continuous Learning Pipeline from inadvertently weakening safety constraints.

## 1. The 6-Layer Architecture

| Layer | Name | Purpose | Content | Update Frequency | Immutable? |
|---|---|---|---|---|---|
| **Layer 0** | Identity | Agent identity | Name, role, bank identity, capabilities, limitations | Quarterly or on rebrand | ✅ Yes (by learning pipeline) |
| **Layer 1** | Safety | Immutable safety rules | Financial advice prohibition, PII handling, auth requirements, prohibited actions | Annual (requires RC approval) | ✅ Yes (by learning pipeline) |
| **Layer 2** | Regulatory | Compliance boundaries | RBI rules, PCI DSS requirements, KYC/AML constraints, mandatory disclosures | As regulations change (fast-track) | ✅ Yes (by learning pipeline) |
| **Layer 3** | Dialogue | Conversation management | Tone guidelines, empathy patterns, confirmation protocols, escalation conditions | Monthly | ❌ No (tunable via LoRA) |
| **Layer 4** | Knowledge | RAG instructions | How to use retrieved knowledge, citation requirements, uncertainty handling | Monthly | ❌ No (tunable via LoRA) |
| **Layer 5** | Operational | Dynamic context | Current promotions, system status, known issues, temporary policy changes | Daily or as needed | ❌ No (no RC review needed) |

### Immutability Rules
- **Layers 0-2** are **immutable** by the Continuous Learning Pipeline. Only Layers 3-5 are modifiable through continuous improvement.
- Only **Layer 5** can be updated without a Release Committee (RC) review.
- The system prompt is versioned. Every prompt version is stored with metadata: `author`, `approval_chain`, `deployment_date`, `model_version`.

## 2. Design Principles

1. **Explicit Over Implicit**: Every safety constraint is explicitly stated. Never rely on the LLM's implicit understanding of banking behavior.
2. **Positive and Negative Instruction**: For each safety boundary, specify both what the agent SHOULD do and what it MUST NOT do.
3. **Testable Assertions**: Every instruction must be convertible to a testable assertion. If it cannot be tested, it is too vague.
4. **Context-Aware Activation**: Support conditional activation of instruction sets based on conversation context (e.g., loan-specific instructions activate only when `intent_domain == "Loan Guidelines"`).
5. **Version Control**: Every prompt version stored with metadata (author, approval chain, deployment date, model version).

## 3. Configuration Parameters

These parameters control the agent's behavior and are tunable within defined ranges. Changes require specific approval processes.

| Configuration | Default | Range | Owner | Change Process |
|---|---|---|---|---|
| `intent_confidence_threshold` | 0.70 | 0.50 – 0.95 | NLU Team | A/B test required |
| `escalation_sentiment_threshold` | -0.70 | -1.0 – -0.3 | CX Team | CEB approval |
| `max_conversation_turns` | 25 | 10 – 50 | Product | Standard change |
| `session_timeout_minutes` | 5 | 3 – 15 | Security | RC approval |
| `knowledge_retrieval_top_k` | 5 | 3 – 10 | NLU Team | A/B test required |
| `retrieval_confidence_threshold` | 0.65 | 0.50 – 0.90 | NLU Team | A/B test required |
| `guardrail_strictness_level` | HIGH | MED / HIGH / CRIT | RC | RC approval required |
| `model_temperature` | 0.3 | 0.0 – 1.0 | ML Team | A/B test + RC review |

## 4. Full Prompt Skeleton

Below is a simplified representation of how the 6 layers are assembled into the final system prompt sent to the LLM at runtime.

```
╔══════════════════════════════════════════════════════╗
║  LAYER 0: IDENTITY                                   ║
║  You are NexBot, the official AI assistant for        ║
║  NexBank. You serve 2.7M customers across India.      ║
╠══════════════════════════════════════════════════════╣
║  LAYER 1: SAFETY (IMMUTABLE)                         ║
║  - You MUST NOT provide financial advice.             ║
║  - You MUST NOT predict market movements.             ║
║  - You MUST NOT output unmasked PII.                  ║
║  - You MUST append the legal disclaimer when...       ║
╠══════════════════════════════════════════════════════╣
║  LAYER 2: REGULATORY (IMMUTABLE)                     ║
║  - Always disclose APR, fees, and penal charges...    ║
║  - Never store or request full card PAN or CVV.       ║
║  - If KYC is expired, block product onboarding.       ║
╠══════════════════════════════════════════════════════╣
║  LAYER 3: DIALOGUE                                    ║
║  - Use an empathetic, professional tone.              ║
║  - Confirm critical actions before executing.         ║
║  - If sentiment < -0.7 for 2 turns, escalate.        ║
╠══════════════════════════════════════════════════════╣
║  LAYER 4: KNOWLEDGE (RAG)                             ║
║  - Base answers ONLY on retrieved KB chunks.          ║
║  - If retrieval confidence < 0.65, do NOT answer.     ║
║  - Cite the source document when providing rates.     ║
╠══════════════════════════════════════════════════════╣
║  LAYER 5: OPERATIONAL (DYNAMIC)                       ║
║  - Current promo: 0% processing fee on Home Loans.    ║
║  - Known issue: UPI refunds delayed by 24hrs.         ║
║  - System status: All services operational.           ║
╚══════════════════════════════════════════════════════╝
```
