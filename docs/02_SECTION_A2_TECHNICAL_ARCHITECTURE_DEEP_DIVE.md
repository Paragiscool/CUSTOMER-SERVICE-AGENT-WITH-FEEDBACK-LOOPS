SECTION A2: TECHNICAL ARCHITECTURE DEEP DIVE 
A2.1 Dialogue Management Engine 
The dialogue management engine is the central orchestrator. Candidates must specify: complete 
dialogue state schema tracking current state of every active conversation including current intent 
classification with confidence score and alternative hypotheses; entity slot-filling status for each 
required and optional slot; conversation history buffer with configurable depth (minimum 20 
turns); user sentiment trajectory across the conversation; authentication level (anonymous, 
OTP-verified, biometric-verified, full KYC); active guardrail state; escalation proximity score; 
and context carry-over flags for multi-session continuity. 
The dialogue policy must address: action space definition (respond, clarify, confirm, escalate, 
transfer, hold, recommend, apologise); policy decision logic (rule-based, learned, or hybrid); 
confirmation strategies for critical information; clarification strategies for ambiguous inputs; 
recovery strategies for misunderstandings; and proactive strategies for helpful suggestions. 
Multi-turn reasoning must cover: anaphora resolution, topic switching detection, implicit intent 
recognition, contradiction handling, and context window management for conversations 
exceeding capacity. 
 
A2.2 Natural Language Understanding (NLU) Pipeline 
The NLU pipeline must include a hierarchical intent classification system with: primary intent 
layer (account, transaction, product, complaint, security, general); secondary intent layer (e.g., 
account > balance_check, account > statement_request); tertiary intent layer (e.g., transaction > 
dispute > merchant_error vs duplicate_charge); confidence thresholds for each layer; multi-intent 
detection; and out-of-scope detection. 
Entity extraction must define types, extraction methods, and validation rules: 
Entity Type 
Examples 
Validation 
Sensitivity 
Account Number 
SA-XXXX-XXXX-1
234 
Regex + Luhn + 
verification 
HIGH - PII 
Transaction Amount 
₹1,500.00, $200 
Currency detect + 
range check 
MEDIUM 
Card Number 
(Partial) 
Last 4 digits: 5678 
Format validation, 
never full 
CRITICAL - PCI 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 6 


## Page 7

 
 
 
 
Aadhaar (Partial) 
Last 4 digits only 
Never accept full 
Aadhaar 
CRITICAL - KYC 
PAN Number 
ABCDE1234F 
Format + NSDL 
verification 
CRITICAL - KYC 
UPI ID 
user@bank 
Format + handle 
resolution 
MEDIUM 
Phone Number 
+91 98765 43210 
Format + country 
code 
HIGH - PII 
Merchant Name 
Amazon, Swiggy 
Fuzzy match against 
DB 
LOW 
Sentiment analysis must include: real-time continuous scoring, emotion classification 
(frustration, anger, confusion, satisfaction, urgency, anxiety, resignation), sentiment trajectory 
tracking, cultural context sensitivity for Indian English, and sarcasm/irony detection. 
 
A2.3 Knowledge Base Architecture 
The knowledge base must cover: knowledge categories (product info, policies, regulatory 
requirements, FAQ, troubleshooting, escalation procedures); representation formats (structured 
key-value, semi-structured JSON/YAML, unstructured NL passages); version control for live 
updates without interruption; regulatory knowledge tagging with compliance metadata; TTL 
specifications for freshness management; and access control by authentication level. 
Retrieval architecture must specify: dense retrieval (embedding-based), sparse retrieval 
(BM25/TF-IDF), or hybrid with fusion weights; cross-encoder re-ranking; contextual retrieval 
query modification; confidence scoring with uncertainty acknowledgment threshold; maximum 
200ms latency at 95th percentile; and graceful fallback when no relevant knowledge is found. 
 
A2.4 Response Generation Architecture 
Generation strategy must define: template-based generation for safety-critical responses (account 
balances, transaction details, regulatory information); LLM-based generation for conversational 
responses (empathy statements, clarifications); hybrid blending approach; persona consistency 
for NexBank brand voice; and response length calibration by context. 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 7 


## Page 8

 
 
 
 
The response safety layer must verify: financial accuracy against authoritative sources; 
regulatory compliance scan; PII leak detection; hallucination detection blocking ungrounded 
information; tone verification for empathy and professionalism; and commitment detection 
flagging unfulfillable promises. 
Multi-language support must cover: Hindi, English, and Hinglish as primary; Tamil, Telugu, 
Bengali, Marathi, Gujarati as secondary; language detection from initial messages; 
code-switching handling; translation pipeline architecture; and cultural adaptation for different 
linguistic communities. 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 8 


## Page 9

 
 
 
 
