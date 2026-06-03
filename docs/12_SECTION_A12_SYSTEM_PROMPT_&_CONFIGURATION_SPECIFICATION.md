SECTION A12: SYSTEM PROMPT & CONFIGURATION SPECIFICATION 
A12.1 System Prompt Architecture (6 Layers) 
Layer 
Purpose 
Content 
Update Frequency 
Layer 0: 
Identity 
Agent 
identity 
Name, role, bank identity, 
capabilities, limitations 
Quarterly or on rebrand 
Layer 1: 
Safety 
Immutable 
safety 
Financial advice prohibition, PII 
handling, auth requirements, 
prohibited actions 
Annual (requires RC 
approval) 
Layer 2: 
Regulatory 
Compliance 
boundaries 
RBI rules, PCI DSS requirements, 
KYC/AML constraints, 
disclosures 
As regulations change 
(fast-track) 
Layer 3: 
Dialogue 
Conversation 
management 
Tone guidelines, empathy 
patterns, confirmation protocols, 
escalation conditions 
Monthly 
Layer 4: 
Knowledge 
RAG 
instructions 
How to use retrieved knowledge, 
citation requirements, uncertainty 
handling 
Monthly 
Layer 5: 
Operation
al 
Dynamic 
context 
Current promotions, system status, 
known issues, temporary policy 
changes 
Daily or as needed 
 
A12.2 System Prompt Design Principles 
●​ Layered Immutability: Layers 0-2 immutable by learning pipeline. Only Layers 3-5 
modifiable through continuous improvement; only Layer 5 without RC review. 
●​ Explicit Over Implicit: Every safety constraint explicitly stated. Never rely on LLM 
implicit understanding of banking behaviour. 
●​ Positive and Negative Instruction: For each safety boundary, specify both SHOULD do 
and MUST NOT do. 
●​ Testable Assertions: Every instruction convertible to a testable assertion. If it cannot be 
tested, it is too vague. 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 33 


## Page 34

 
 
 
 
●​ Context-Aware Activation: Support conditional activation of instruction sets based on 
conversation context. 
●​ Version Control: Every prompt version stored with metadata (author, approval chain, 
deployment date, model version). 
 
A12.3 Configuration Parameters 
Configuration 
Default 
Range 
Owner 
Change Process 
intent_confidence_thresho
ld 
0.70 
0.50-0.95 NLU Team 
A/B test required 
escalation_sentiment_thre
shold 
-0.70 
-1.0 to 
-0.3 
CX Team 
CEB approval 
max_conversation_turns 
25 
10-50 
Product 
Standard change 
session_timeout_minutes 
5 
3-15 
Security 
RC approval 
knowledge_retrieval_top_
k 
5 
3-10 
NLU Team 
A/B test required 
retrieval_confidence_thres
hold 
0.65 
0.50-0.90 NLU Team 
A/B test required 
guardrail_strictness_level 
HIGH 
MED/HI
GH/CRI
T 
RC 
RC approval required 
model_temperature 
0.3 
0.0-1.0 
ML Team 
A/B test + RC review 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 34 


## Page 35

 
 
 
 
