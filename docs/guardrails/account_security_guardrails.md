# Account Security Guardrails

To protect customer assets and prevent unauthorized account modifications, the Agentic AI must strictly enforce the following 8 Mandatory Account Security Guardrails before executing any state-changing tool call.

## 1. High-Value Transaction MFA (Step-Up Authentication)
- **Trigger**: Any outward fund transfer exceeding ₹50,000 (or equivalent local threshold), or cumulative daily transfers exceeding ₹100,000.
- **Rule**: The agent cannot execute the transfer API without first initiating and verifying a time-bound, out-of-band One-Time Password (OTP) sent to the registered mobile number.

## 2. Unrecognized Device / Location Anomaly
- **Trigger**: A session initiated from a new device fingerprint, an IP address outside the user's historical geographical radius, or impossible travel velocity (e.g., login from Mumbai, followed by a login from London 10 minutes later).
- **Rule**: The agent is restricted to read-only capabilities (balance checks, general info). All transaction APIs and profile modification APIs are blocked until the user completes a strong secondary verification (e.g., in-app biometric approval).

## 3. Beneficiary Cool-down Period
- **Trigger**: Addition of a new payee/beneficiary.
- **Rule**: For the first 24 hours after a new beneficiary is added, the agent is hard-coded to reject any transfer to that beneficiary exceeding a minimal "trust" threshold (e.g., ₹10,000).

## 4. Concurrent Session Invalidation
- **Trigger**: The dialogue management engine detects active polling from two distinct session tokens for the same user ID simultaneously.
- **Rule**: The agent instantly terminates both sessions, forces a global logout, and requires the user to re-authenticate with MFA.

## 5. PII Masking and Retention Limits
- **Trigger**: Displaying sensitive account details (Card numbers, Aadhaar, PAN) back to the user in the chat UI.
- **Rule**: The agent must apply strict masking (e.g., `XXXX-XXXX-XXXX-1234`). Furthermore, the chat session transcript must never store the unmasked variants in the persistent database.

## 6. Hard Daily Transfer Limits
- **Trigger**: A requested transaction exceeds the user's defined daily transaction limit.
- **Rule**: The agent must unconditionally reject the transaction. It cannot be overridden by social engineering or urgency. The user must use the self-service portal to change their global limits (which triggers Guardrail 8).

## 7. Suspicious Pattern Lock (Brute Force Defense)
- **Trigger**: 3 consecutive failed authentication attempts (e.g., incorrect PIN, wrong OTP) or 5 failed knowledge-based verification questions in a single session.
- **Rule**: The account is temporarily locked for 30 minutes, and the agent is disabled for that user. An alert is sent to the fraud monitoring team.

## 8. Biometric Re-Auth for Profile Modifications
- **Trigger**: Requests to change the registered email address, mobile phone number, or physical mailing address.
- **Rule**: The agent cannot process this request via text alone. It must trigger a deep link to the mobile app requiring a biometric authentication (Face ID / Fingerprint) matched against the secure enclave, preventing social-engineered account takeovers.
