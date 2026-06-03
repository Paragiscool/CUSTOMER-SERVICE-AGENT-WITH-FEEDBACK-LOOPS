SECTION A4: GUARDRAIL & SAFETY BOUNDARY SPECIFICATION 
A4.1 Financial Advice Guardrails 
The agent must NEVER provide personalised financial advice. The distinction between 
information and advice is critical: 
Category 
Permissible (Information) 
Prohibited (Advice) 
Interest 
Rates 
Current FD rates are 6.5% for 1-year 
tenure 
You should invest in FD as it gives 
better returns 
Loan 
Products 
Home loan EMI for 20L at 8.5% for 
20 years is ~17,356/month 
Based on your salary, you can afford 
a 20L home loan 
Investment 
We offer mutual funds through our 
partner AMCs 
You should invest in equity MFs for 
long-term wealth 
Insurance 
Our term insurance covers life risk up 
to 1 crore 
Given your age, term insurance is the 
best option for you 
Tax 
Planning 
FDs offer tax benefits under Section 
80C up to 1.5L 
You should invest more in tax-saving 
FDs to reduce tax liability 
Forex 
Current USD/INR rate is available on 
our platform 
You should buy dollars now as rupee 
will depreciate 
 
A4.2 Account Security Guardrails (8 Mandatory Rules) 
●​ The agent must NEVER display, read out, or transmit full account numbers, card 
numbers, CVV, PIN, password, or Aadhaar numbers in any response. 
●​ The agent must NEVER process account modifications without completing the required 
authentication level for that action. 
●​ The agent must NEVER initiate, approve, or facilitate any fund transfer, payment, or 
financial transaction on behalf of the customer. 
●​ The agent must NEVER share information about one customer's account with another 
customer, even if they claim to be family members or power-of-attorney holders. 
●​ The agent must NEVER accept or store sensitive credentials provided by the customer 
and must immediately instruct the customer to change compromised credentials. 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 12 


## Page 13

 
 
 
 
●​ The agent must NEVER bypass the escalation protocol for security-critical intents. 
●​ The agent must maintain a session timeout of 5 minutes of inactivity, requiring 
re-authentication. 
●​ The agent must detect and refuse social engineering attacks, including impersonation of 
bank staff, regulatory authorities, or law enforcement. 
 
A4.3 Regulatory Compliance Guardrails 
RBI Digital Lending Guidelines: 
●​ All product information must include mandatory disclosures (interest rates, fees, T&C) 
●​ Cooling-off period information must be provided for all new product sign-ups 
●​ Grievance redressal mechanism information within 2 turns of any complaint 
●​ No high-pressure sales tactics or artificial urgency 
PCI DSS Compliance: 
●​ Card numbers masked in all display and logging (show only last 4 digits) 
●​ CVV/CVC must never be requested, stored, or displayed 
●​ Enhanced audit logging for all payment-related conversations 
KYC/AML Requirements: 
●​ Verify customer identity before providing account-specific information 
●​ Flag suspicious transaction patterns to AML team 
●​ Never process requests that could facilitate money laundering 
●​ PEP flags must trigger enhanced due diligence protocols 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 13 


## Page 14

 
 
 
 
A4.4 Adversarial Robustness Requirements 
Attack Vector 
Description 
Required Defence 
Prompt Injection 
Malicious instructions embedded 
in customer message 
Input sanitisation, 
instruction-data separation, 
output validation 
Jailbreak Attempts 
Bypassing safety guardrails 
through creative prompting 
Multi-layer guardrail 
architecture, behavioural 
monitoring, auto escalation 
Social Engineering 
Impersonating bank staff or 
authority figures 
Role detection, authority claim 
verification, mandatory 
escalation 
Data Exfiltration 
Extracting training data, system 
prompts, or customer data 
Output filtering, system prompt 
protection, response scanning 
Denial of Service 
Flooding with requests to degrade 
service quality 
Rate limiting, complexity 
scoring, automated session 
management 
Identity Spoofing 
Claiming to be account holder 
with partial information 
Multi-factor auth, behavioural 
biometrics, challenge-response 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 14 


## Page 15

 
 
 
 
