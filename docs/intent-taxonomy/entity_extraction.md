# Entity Extraction Specification

This document defines the entity types to be extracted by the NLU Pipeline, including extraction rules, validation constraints, and safety/sensitivity ratings.

## 1. Core Entity Types & Validation Constraints

| Entity Type | Description | Extraction Strategy | Validation Rules | Sensitivity |
| :--- | :--- | :--- | :--- | :--- |
| `account_id` | Full Account Number | Regex `\b[0-9]{10,14}\b` | Passes Luhn algorithm check; must match user's profile | **HIGH** (PII) |
| `account_last_4` | Partial Account Number | Regex `\b[0-9]{4}\b` | Must match end of a registered account | MEDIUM |
| `account_type` | Type of account | List: `[savings, current, fd, rd, loan]` | Must match canonical types | LOW |
| `card_last_4` | Partial Card Number | Regex `\b[0-9]{4}\b` | Must match end of a registered card | **CRITICAL** (PCI) |
| `amount` | Monetary value | NER / Regex parsing | Must be valid numeric > 0; convert to float | MEDIUM |
| `currency` | Currency symbol/code | NER / List `[INR, USD, EUR]` | Must map to ISO 4217 | LOW |
| `date_range` | Date or Time Window | Custom date parser (e.g., Duckling) | Ensure end_date >= start_date | LOW |
| `transaction_id` | Specific Tx Reference | Regex `\bTXN-[A-Z0-9]{8,12}\b` | Check against Core Banking records | HIGH |
| `upi_ref` | UPI Reference Number | Regex `\b[0-9]{12}\b` | Format check | MEDIUM |
| `beneficiary` | Name or UPI ID of payee | NER / Regex `.+@.+` | Length constraints | MEDIUM |
| `merchant_name` | Name of merchant | NER (Organization) | Fuzzy match against known merchant DB | LOW |
| `email` | Customer email | Regex standard email pattern | Valid syntax check | **HIGH** (PII) |
| `phone_number` | Customer phone | Regex `\+?[0-9]{10,12}` | Must have 10 digits for Indian numbers | **HIGH** (PII) |
| `aadhaar_last_4`| Partial Aadhaar | Regex `\b[0-9]{4}\b` | Never accept full Aadhaar (12 digits) | **CRITICAL** (KYC) |
| `pan_number` | PAN Card Number | Regex `[A-Z]{5}[0-9]{4}[A-Z]{1}` | Format check; verify via NSDL API | **CRITICAL** (KYC) |

## 2. Slot Filling & Orchestration Rules

When an intent is detected but required entities are missing, the Dialogue Management Engine (DME) enters the `SLOT_FILLING` state.

### 2.1 Missing Entity Prompting
- **Strategy**: Ask for exactly one missing required entity at a time.
- **Max Retries**: 3 retries per entity. If the user fails to provide a valid entity after 3 attempts, escalate to `HUMAN_AGENT`.
- **Defaults**: If an optional entity is missing, proceed with a system default if applicable (e.g., if `currency` is missing, default to `INR`).

### 2.2 Entity Confirmation Strategies
- **Implicit Confirmation**: Used for low-sensitivity intents. The agent includes the extracted entity in the subsequent response.
  * *Example: "Checking the balance for your savings account..."*
- **Explicit Confirmation**: Used for HIGH and CRITICAL sensitivity intents (e.g., fund transfers, address updates). The agent requires a direct "Yes" before proceeding.
  * *Example: "You want to block your card ending in 4567. Please reply 'Yes' to confirm."*

### 2.3 Context Carry-Over
Entities extracted in a previous turn remain in the short-term context buffer (20 turns). If a new intent requires an entity that is already in the buffer (e.g., the user previously mentioned their `card_last_4`), the DME will auto-fill that slot.

## 3. Negative Constraints (Anti-Patterns)
- **Full Card Numbers**: If the NLU detects a full 16-digit card number or a 3-digit CVV, it must immediately scrub the value from memory, log an alert, and instruct the user never to share full card details.
- **Full Aadhaar**: If the NLU detects a full 12-digit Aadhaar number, it must mask all but the last 4 digits instantly and trigger a data-spillage audit event.
