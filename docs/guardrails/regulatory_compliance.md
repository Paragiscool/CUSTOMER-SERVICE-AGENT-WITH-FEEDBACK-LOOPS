# Regulatory Compliance Guardrails

The NexBank Agentic AI operates in a heavily regulated environment. Technical controls are mapped directly to corresponding statutory and regulatory frameworks to ensure continuous compliance.

## 1. Reserve Bank of India (RBI) Guidelines
The system enforces strict compliance with RBI directives regarding digital banking, lending, and customer protection.
- **Digital Lending Transparency**: The agent must always provide the exact Annual Percentage Rate (APR), processing fees, and penal charges upfront when discussing loan products, without the user explicitly asking for them. (Mapped to RBI Digital Lending Circular).
- **Data Localization**: All conversational data, vector embeddings, and dialogue states must be processed and stored exclusively in data centers physically located within India.
- **Grievance Redressal**: The agent must provide the contact details of the Principal Nodal Officer without hesitation when a user expresses dissatisfaction or explicitly asks to escalate a complaint.

## 2. Payment Card Industry Data Security Standard (PCI DSS)
The agent interacts with card management systems and must not violate PCI compliance.
- **Zero Storage of PAN/CVV**: The system is prohibited from asking for, parsing, or storing the full Primary Account Number (PAN) or the CVV. If a user accidentally pastes a full card number, the Ingestion Gate immediately tokenizes it before it reaches the LLM or any database.
- **Encrypted Transmission**: All API calls from the Dialogue Management Engine to the Core Banking System (for card blocking/unblocking) use mutual TLS (mTLS) with payload-level encryption.

## 3. Know Your Customer (KYC) / Anti-Money Laundering (AML)
The agent acts as a first line of defense against financial crimes.
- **KYC Status Verification**: Before facilitating any new product onboarding (e.g., opening a new account variant), the agent must query the central KYC registry. If the user's Video-KYC or physical KYC is expired, the flow is blocked.
- **Suspicious Activity Reporting (SAR) Triggers**: If a user's conversational pattern indicates structuring (e.g., asking "What is the exact threshold before a transaction is reported to the tax authorities?") or attempts to bypass source-of-funds declarations, the agent silently flags the session for the AML investigation team without alerting the user.

## 4. Digital Personal Data Protection (DPDP) Act
The system handles personal data in accordance with the DPDP Act 2023.
- **Consent Architecture**: The agent cannot use personal financial data (like spending habits) to recommend products unless explicit, granular consent has been recorded in the user's profile.
- **Right to be Forgotten**: If a user invokes their right to data erasure, the agent triggers an automated workflow that purges their conversational history from the Vector DB and logs, while legally retaining transactional audit trails as required by the RBI.
