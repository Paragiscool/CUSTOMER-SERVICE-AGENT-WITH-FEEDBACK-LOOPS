## Page 1

 
 
 
 
AGENTIC AI - CUSTOMER SERVICE AGENT WITH 
FEEDBACK LOOPS 
Category: Customer Service Agent | AI Application Technology | Agentic AI Engineer 
Timeline: 15 Days | AI-Accelerated Innovation & Research Project  
Target: Undergraduate / MBA / Engineering Students  
 
TASK: 
Design a production-grade, self-improving agentic AI customer service system for NexBank - a 
fictional neo-bank with 2.7 million active customers in India - that autonomously handles account 
queries, transaction disputes, product recommendations, and complaint resolution across 18,000 
daily customer interactions, while incorporating continuous self-improvement mechanisms that 
learn from supervisor corrections, customer satisfaction scores, and resolution outcomes. The 
system must include a complete conversational architecture with dialogue state management and 
multi-turn reasoning, a 25+ intent taxonomy with hierarchical NLU classification, a dynamic 
knowledge base with sub-200ms retrieval, comprehensive safety guardrails that prevent incorrect 
financial advice and unauthorised account changes, a human-in-the-loop escalation framework 
with 10+ trigger conditions, and a continuous learning pipeline that improves performance over 
time without degrading safety invariants. 
 
PART A: TRAINING MATERIAL & LEARNING RESOURCES 
PART B: GAMIFIED SIMULATION BUSINESS CONCEPT 
PART C: CASE STUDIES & REAL-WORLD APPLICATIONS 
PART D: 15-DAY PROJECT METHODOLOGY & DELIVERABLES 
PART E: NO-CODE TOOLS & PLATFORMS GUIDE 
PART F: SUBMISSION PROTOCOL 
 
Important Note: 
This Project is designed, developed, and administered solely by Zetheta Algorithms Private Limited 
("Zetheta") for the purpose of assessing candidates, students, or event participants on behalf of engaging 
employers, institutes, or event organisers. Completion of this Project does not guarantee employment, 
academic outcomes, awards, or any other result - all such decisions remain at the sole discretion of the 
respective engaging party. Zetheta may use AI-assisted tools to evaluate submissions; such outputs are 
indicative in nature and may be inaccurate or incomplete. By participating, you consent to your 
performance data, scores, and submissions being shared with relevant engaging parties for assessment and 
decision-making purposes. 
 
 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 1 


## Page 2

 
 
 
 
EXECUTIVE SUMMARY 
This project positions students as Conversational AI Architects at a fictional fast-growing 
neo-bank - NexBank - requiring them to master the convergence of dialogue systems 
engineering, safety-critical AI design, and continuous learning pipeline architecture within the 
highly regulated domain of digital banking. Unlike typical AI projects that treat safety as an 
afterthought, this project demands simultaneous fluency in natural language understanding, 
multi-turn context management, and knowledge retrieval architecture on one side, and RBI 
regulatory compliance, financial advice guardrails, and adversarial robustness on the other - with 
a project complexity rating of 9.8 out of 10 for safety and compliance rigour alone. 
The core challenge students must solve: How can a neo-bank replace 60% of its first-line human 
support - handling everything from routine balance queries to high-stakes fraud alerts and loan 
restructuring requests - with an AI system that is not only conversationally capable but also 
provably safe, continuously improving, and satisfying to both the Customer Experience Board 
that demands a 4.5/5.0 satisfaction score and a Risk Committee that tolerates zero incorrect 
financial advice errors? The answer lies in a layered architecture spanning a dialogue 
management engine with full conversation state tracking, a hierarchical NLU pipeline with 
three-tier intent classification, a guardrail system covering 25+ intent categories and six 
adversarial attack vectors, and a self-improvement loop that integrates human supervisor 
corrections, automated quality metrics, and A/B testing frameworks while preserving safety 
invariants across every model update cycle. 
Students will design and validate their system against 500 real customer interaction transcripts 
across 10 interaction categories - including transaction disputes averaging 8.7 conversational 
turns, complaint resolutions with just 58% historical resolution rates, and fraud alerts requiring 
critical-priority handling - progressing through seven escalating design challenges evaluated 
simultaneously by a Customer Experience Board and a Risk Committee that represent the 
competing governance pressures every production-grade financial AI system must satisfy. The 
system must demonstrate measurable improvement over the legacy call centre baseline across 
resolution rates, average handling time, satisfaction scores, and safety metric performance. 
India's digital banking market is projected to serve over 900 million internet users by 2030, while 
the global conversational AI market in financial services is expected to exceed $6 billion 
annually - yet the vast majority of deployed banking chatbots remain intent-matching rule 
engines incapable of multi-turn reasoning, self-correction, or safe handling of high-stakes 
financial requests. The gap between a basic FAQ bot and a genuinely production-ready financial 
AI agent represents one of the most technically demanding and commercially valuable unsolved 
problems in applied AI today. This project closes that gap - equipping students with the system 
design expertise, safety engineering discipline, and continuous learning architecture skills 
directly applicable to conversational AI roles at neo-banks, digital lending platforms, payments 
companies, and financial technology firms building the next generation of AI-first customer 
experiences. 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 2 


## Page 3

 
 
 
 
PART A: TRAINING MATERIAL & LEARNING RESOURCES 
SECTION A1: DOMAIN FUNDAMENTALS - AI IN BANKING CUSTOMER SERVICE 
A1.1 The NexBank Scenario 
You have been appointed as the Conversational AI Architect at NexBank, a rapidly growing 
neo-bank headquartered in Mumbai with 2.7 million active customers across India. NexBank has 
been operating with an outsourced call centre of 450 agents handling approximately 18,000 
customer interactions daily. The board of directors has approved a strategic initiative to replace 
60% of first-line support with an AI-first customer service system within 18 months, projecting 
annual savings of INR 42 crore while simultaneously improving customer satisfaction scores 
from the current 3.2/5.0 to a target of 4.5/5.0. 
You report to the Chief Technology Officer and work closely with three key stakeholder groups: 
the Product Engineering team responsible for banking platform APIs, the Risk & Compliance 
division that must approve all customer-facing AI interactions, and the Customer Experience 
team that owns satisfaction metrics and brand perception. Your role is to design the complete 
conversational AI system architecture - not to implement it in code, but to produce specifications 
of sufficient detail that a team of ML engineers and platform developers could build the system 
from your documents alone. 
 
A1.2 The Data Package 
You have been provided with a simulated dataset of 500 customer interaction transcripts with 
outcome labels. These transcripts span the following interaction categories and must inform your 
design decisions: 
Category 
Count 
Avg 
Turns 
Resolution 
CSAT 
Priority 
Account Balance & 
Statement 
85 
4.2 
94% 
4.1 
LOW 
Transaction Dispute & 
Chargeback 
72 
8.7 
67% 
2.8 
HIGH 
Product Recommendation 
58 
6.1 
42% 
3.5 
MEDIUM 
Complaint Resolution & 
Escalation 
64 
11.3 
58% 
2.4 
CRITICAL 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 3 


## Page 4

 
 
 
 
KYC Update & 
Documents 
45 
5.8 
81% 
3.3 
MEDIUM 
Card Blocking & 
Replacement 
38 
3.4 
96% 
4.3 
LOW 
Loan Enquiry & EMI 
Restructuring 
42 
7.9 
55% 
3.0 
HIGH 
UPI / NEFT / RTGS 
Payment Issues 
51 
5.2 
78% 
3.1 
MEDIUM 
Fixed Deposit & 
Investment 
28 
6.5 
71% 
3.4 
MEDIUM 
Fraud Alert & Account 
Security 
17 
9.1 
89% 
3.7 
CRITICAL 
 
A1.3 Governance Structure 
Customer Experience Board (CEB) 
The CEB comprises the Head of Customer Experience, a UX Research Lead, and a Brand 
Communications Director. Their primary concerns include: conversation naturalness and 
empathy quotient across all interaction types; first-contact resolution rates and the reduction of 
unnecessary escalations; brand voice consistency and adherence to NexBank's 'friendly, 
trustworthy, modern' personality guidelines; accessibility compliance for users with varying 
literacy levels and language preferences; and proactive service capabilities that anticipate 
customer needs before they articulate them. 
Risk Committee (RC) 
The RC comprises the Chief Risk Officer, a Regulatory Compliance Manager, and an 
Information Security Officer. Their primary concerns include: prevention of incorrect financial 
advice that could expose the bank to regulatory penalties or customer losses; prohibition of 
unauthorised account modifications, fund transfers, or data disclosures; compliance with RBI 
digital lending guidelines, PCI DSS data handling requirements, and KYC/AML regulations; 
audit trail completeness for every AI-customer interaction with tamper-proof logging; fail-safe 
mechanisms that default to human escalation when confidence thresholds are not met; and 
adversarial robustness against prompt injection, social engineering, and manipulation attempts. 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 4 


## Page 5

 
 
 
 
A1.4 Competitive Intelligence Briefing 
NexBank's competitors have deployed varying levels of AI customer service. Your design must 
demonstrate awareness of industry best practices while proposing innovations that differentiate 
NexBank: 
Competitor 
Current System 
Weakness 
Competitor A 
(Large Private 
Bank) 
Rules-based chatbot: 12 intents, 
35% containment 
'Talking to a wall' complaints; no 
NLU sophistication 
Competitor B 
(Neo-bank) 
LLM-powered assistant: 22 
intents, 52% containment 
Regulatory warning for 
unsolicited investment advice 
Competitor C 
(Payment Platform) 
Hybrid system: 18 intents, 48% 
containment 
Strong on payments, weak on 
complex financial products 
Industry 
Benchmark 
(Global) 
AI-first banks: 65% containment, 
4.0+ CSAT 
<2% incorrect advice rate 
benchmark 
Your design targets: 70%+ containment rate, 4.5+ CSAT, 0% incorrect financial advice rate, 
<30-second first reply. 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 5 


## Page 6

 
 
 
 
