SECTION A3: INTENT TAXONOMY SPECIFICATION (25+ CATEGORIES) 
Candidates must design a comprehensive intent taxonomy covering the full spectrum of 
customer interactions at NexBank. The minimum 25 intent categories are specified below. 
Candidates are expected to expand to 30+ categories with sub-intents, entity requirements, and 
disambiguation rules. 
A3.1 Account Management Intents (7 categories) 
ID 
Intent Name 
Required Entities 
Auth Level 
Safety 
ACC-00
1 
Check Account 
Balance 
account_id OR 
account_type 
OTP-Verified 
LOW 
ACC-00
2 
Request Account 
Statement 
account_id, 
date_range, format 
OTP-Verified 
LOW 
ACC-00
3 
Update Contact 
Details 
field_to_update, 
new_value 
Biometric 
HIGH 
ACC-00
4 
Update Mailing 
Address 
new_address, 
proof_document 
Biometric 
HIGH 
ACC-00
5 
Account Closure 
Request 
account_id, reason, 
confirmation 
Full KYC + 
Branch 
CRITICAL 
ACC-00
6 
Nominee Update 
nominee_name, 
relationship, id_proof 
Full KYC 
CRITICAL 
ACC-00
7 
Account 
Upgrade/Downgrade 
current_type, 
desired_type 
OTP-Verified 
MEDIUM 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 9 


## Page 10

 
 
 
 
A3.2 Transaction & Payment Intents (6 categories) 
ID 
Intent Name 
Required Entities 
Auth Level 
Safety 
TXN-00
1 
Transaction Status 
Enquiry 
transaction_id OR 
description 
OTP-Verified 
LOW 
TXN-00
2 
Raise Transaction 
Dispute 
transaction_id, 
dispute_reason, 
amount 
OTP-Verified 
HIGH 
TXN-00
3 
UPI Payment Failure 
upi_ref, beneficiary, 
amount, timestamp 
OTP-Verified 
MEDIUM 
TXN-00
4 
NEFT/RTGS Status 
Check 
ref_number, 
beneficiary_bank 
OTP-Verified 
LOW 
TXN-00
5 
Recurring Payment 
Setup/Cancel 
beneficiary, amount, 
frequency 
Biometric 
HIGH 
TXN-00
6 
International Transfer 
Enquiry 
amount, currency, 
destination_country 
Full KYC 
HIGH 
 
A3.3 Card, Product, Complaint & Security Intents (16+ categories) 
Card Management (CRD-001 to CRD-005): Block/Unblock Card, Card Replacement, Credit 
Limit Change, EMI Conversion, Reward Points Enquiry. Product & Advisory (PRD-001 to 
PRD-005): Product Information, Loan Eligibility, FD/RD Rate Enquiry, Insurance Enquiry, 
Investment Advisory Request. Complaint & Feedback (CMP-001 to CMP-005): Register 
Complaint, Check Status, Escalate, Provide Feedback, Request Supervisor Callback. Security & 
Fraud (SEC-001 to SEC-004): Report Fraud, Report Phishing, Reset Credentials, Suspicious 
Activity Response. Full specifications with entity requirements, authentication levels, and safety 
ratings must be provided for each in your deliverable. 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 10 


## Page 11

 
 
 
 
A3.4 Intent Disambiguation Requirements 
Candidates must specify disambiguation strategies for commonly confused intent pairs 
including: TXN-001 vs TXN-002 (status check vs dispute); PRD-001 vs PRD-005 (information 
vs advisory); ACC-003 vs ACC-004 (contact vs address update); CMP-001 vs CMP-003 (new 
complaint vs escalation); SEC-001 vs TXN-002 (fraud vs merchant dispute); and CRD-001 vs 
SEC-001 (card block vs fraud report). Each disambiguation strategy must include clarifying 
question sequences, confidence threshold logic, and entity-based differentiation rules. 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 11 


## Page 12

 
 
 
 
