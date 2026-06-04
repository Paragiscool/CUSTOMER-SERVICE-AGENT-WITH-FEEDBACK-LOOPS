# Intent Taxonomy Specification

This document defines the **37 hierarchical intent categories** for the NexBank Agentic AI system, covering the full spectrum of customer interactions across 7 primary domains.

## 1. Account Management (ACC)

| ID | Intent Name | Required Entities | Optional Entities | Auth Level | Safety |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **ACC-001** | Check Account Balance | `account_id` OR `account_type` | None | OTP-Verified | LOW |
| **ACC-002** | Request Account Statement | `account_id`, `date_range`, `format` | None | OTP-Verified | LOW |
| **ACC-003** | Update Contact Details | `field_to_update`, `new_value` | None | Biometric | HIGH |
| **ACC-004** | Update Mailing Address | `new_address`, `proof_document` | None | Biometric | HIGH |
| **ACC-005** | Account Closure Request | `account_id`, `reason`, `confirmation` | None | Full KYC + Branch | CRITICAL |
| **ACC-006** | Nominee Update | `nominee_name`, `relationship`, `id_proof` | `nominee_dob` | Full KYC | CRITICAL |
| **ACC-007** | Account Upgrade/Downgrade | `current_type`, `desired_type` | None | OTP-Verified | MEDIUM |

## 2. Transaction & Payment (TXN)

| ID | Intent Name | Required Entities | Optional Entities | Auth Level | Safety |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TXN-001** | Transaction Status Enquiry | `transaction_id` OR `description` | `amount`, `date` | OTP-Verified | LOW |
| **TXN-002** | Raise Transaction Dispute | `transaction_id`, `dispute_reason`, `amount` | `merchant_name` | OTP-Verified | HIGH |
| **TXN-003** | UPI Payment Failure | `upi_ref`, `beneficiary`, `amount`, `timestamp` | None | OTP-Verified | MEDIUM |
| **TXN-004** | NEFT/RTGS Status Check | `ref_number`, `beneficiary_bank` | `amount` | OTP-Verified | LOW |
| **TXN-005** | Recurring Payment Setup/Cancel | `action_type`, `beneficiary`, `amount`, `frequency` | `end_date` | Biometric | HIGH |
| **TXN-006** | International Transfer Enquiry | `amount`, `currency`, `destination_country` | `purpose_code` | Full KYC | HIGH |

## 3. Card Management (CRD)

| ID | Intent Name | Required Entities | Optional Entities | Auth Level | Safety |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **CRD-001** | Block/Unblock Card | `card_last_4`, `action_type`, `reason` | None | OTP-Verified | CRITICAL |
| **CRD-002** | Card Replacement Request | `card_last_4`, `delivery_address` | `reason` | Biometric | HIGH |
| **CRD-003** | Credit Limit Change | `card_last_4`, `desired_limit` | None | Full KYC | HIGH |
| **CRD-004** | EMI Conversion | `transaction_id`, `tenure_months` | `card_last_4` | OTP-Verified | MEDIUM |
| **CRD-005** | Reward Points Enquiry | `card_last_4` | `action_type` (view/redeem) | OTP-Verified | LOW |
| **CRD-006** | PIN Generation/Reset | `card_last_4`, `new_pin_hash` | None | Biometric | CRITICAL |

## 4. Product & Advisory (PRD)

| ID | Intent Name | Required Entities | Optional Entities | Auth Level | Safety |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **PRD-001** | Product Information | `product_type` | `specific_feature` | Anonymous | LOW |
| **PRD-002** | Loan Eligibility Check | `loan_type`, `income_bracket`, `employment_type` | `desired_amount` | OTP-Verified | MEDIUM |
| **PRD-003** | FD/RD Rate Enquiry | `tenure`, `amount` | `customer_category` (e.g., senior) | Anonymous | LOW |
| **PRD-004** | Insurance Enquiry | `insurance_type` | `coverage_amount` | Anonymous | LOW |
| **PRD-005** | Investment Advisory Request | `investment_goal` | `risk_appetite` | OTP-Verified | HIGH |

## 5. Complaint & Feedback (CMP)

| ID | Intent Name | Required Entities | Optional Entities | Auth Level | Safety |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **CMP-001** | Register Complaint | `complaint_category`, `description` | `related_transaction` | Anonymous | MEDIUM |
| **CMP-002** | Check Complaint Status | `complaint_id` | None | OTP-Verified | LOW |
| **CMP-003** | Escalate Complaint | `complaint_id`, `escalation_reason` | None | OTP-Verified | HIGH |
| **CMP-004** | Provide Feedback | `feedback_text`, `rating` | `category` | Anonymous | LOW |
| **CMP-005** | Request Supervisor Callback | `contact_number`, `preferred_time` | `reason` | OTP-Verified | MEDIUM |

## 6. Security & Fraud (SEC)

| ID | Intent Name | Required Entities | Optional Entities | Auth Level | Safety |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **SEC-001** | Report Fraud | `fraud_type`, `description` | `transaction_id` | OTP-Verified | CRITICAL |
| **SEC-002** | Report Phishing | `phishing_source` (email/SMS/call), `details` | None | Anonymous | HIGH |
| **SEC-003** | Reset Credentials | `credential_type` (password/user_id) | None | Biometric | CRITICAL |
| **SEC-004** | Suspicious Activity Response | `alert_id`, `confirmation` (yes/no) | None | Biometric | CRITICAL |

## 7. General & Conversational (GEN)

| ID | Intent Name | Required Entities | Optional Entities | Auth Level | Safety |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **GEN-001** | Greeting | None | None | Anonymous | LOW |
| **GEN-002** | Goodbye | None | None | Anonymous | LOW |
| **GEN-003** | Out of Scope | None | None | Anonymous | LOW |
| **GEN-004** | Human Handoff Request | None | `reason` | Anonymous | MEDIUM |
