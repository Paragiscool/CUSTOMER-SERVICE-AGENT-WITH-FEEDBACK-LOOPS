# Prompt Template Library (15+ Templates)

Each template below is a pre-approved response pattern that the Response Generation Engine (C5) uses. Templates are triggered by specific conditions from the Dialogue Management Engine (C3) and NLU Pipeline (C2).

## Template Index

| # | Template Name | Trigger Condition | Layer |
|---|---|---|---|
| PT-01 | Greeting | Session start / `GEN-001` intent | L3 Dialogue |
| PT-02 | Authentication Request | Auth level insufficient for requested action | L1 Safety |
| PT-03 | Slot-Filling Prompt | Required entity missing from user query | L3 Dialogue |
| PT-04 | Disambiguation | NLU returns 2+ intents with <15% confidence gap | L3 Dialogue |
| PT-05 | Financial Info + Disclaimer | `PRD-*` intent involving rates/products | L1 Safety + L2 Regulatory |
| PT-06 | Advice Deflection | `request_advice` intent detected | L1 Safety |
| PT-07 | Escalation Handoff | Any ESC trigger fires | L3 Dialogue |
| PT-08 | Complaint Acknowledgment | `CMP-001` intent detected | L3 Dialogue |
| PT-09 | Out-of-Scope Deflection | Intent classified as `out_of_scope` | L3 Dialogue |
| PT-10 | Session Timeout Warning | 4 minutes of inactivity detected | L5 Operational |
| PT-11 | After-Hours Notice | Session outside business hours + P1/P2 escalation | L5 Operational |
| PT-12 | Crisis / Self-Harm Response | `ESC-011` trigger | L1 Safety |
| PT-13 | Fraud Report Intake | `SEC-002` intent or `ESC-001` trigger | L1 Safety |
| PT-14 | Product Comparison | User asks to compare 2+ products | L4 Knowledge |
| PT-15 | Loan EMI Calculation | `LN-*` intent with amount + tenure entities | L4 Knowledge |
| PT-16 | Farewell | User signals end of conversation / `GEN-004` | L3 Dialogue |

---

## Template Details

### PT-01: Greeting
**Trigger**: New session initiated, intent `GEN-001`.
```
Hello! 👋 Welcome to NexBank. I'm NexBot, your AI banking assistant. 
I can help you with account inquiries, transactions, card management, 
loan information, and more.

How can I assist you today?
```

### PT-02: Authentication Request
**Trigger**: User requests an action requiring `AUTH_CUSTOMER` or higher, but current session is `Unauthenticated`.
```
To access your account details, I need to verify your identity first. 
Could you please provide your registered mobile number? I'll send 
you a one-time verification code (OTP).
```

### PT-03: Slot-Filling Prompt
**Trigger**: A required entity for the active intent is missing. Dynamic: `{entity_name}` and `{intent_description}` are injected at runtime.
```
To proceed with {intent_description}, I'll need a bit more information. 
Could you please provide your {entity_name}?
```

### PT-04: Disambiguation
**Trigger**: NLU returns 2+ candidate intents with a confidence gap of less than 15%. Dynamic: `{option_1}` and `{option_2}` are injected.
```
I want to make sure I understand you correctly. Did you mean:

1️⃣ {option_1}
2️⃣ {option_2}

Please select the option that best matches your request, or rephrase 
your question.
```

### PT-05: Financial Info + Disclaimer
**Trigger**: Any `PRD-*` (Product & Advisory) intent. The response provides factual information and appends the mandatory legal disclaimer.
```
{factual_response_from_kb}

---
📌 *Please note: I am an AI assistant providing factual information 
about NexBank products. This information does not constitute financial, 
investment, or tax advice. Please consult a certified financial advisor 
for personalized recommendations.*
```

### PT-06: Advice Deflection
**Trigger**: Pre-Generation Intent Classifier detects `request_advice` intent.
```
That's a great question! While I can't make specific recommendations, 
I can provide you with detailed information about the relevant NexBank 
products so you can make an informed decision.

Would you like me to compare the available options for you?
```

### PT-07: Escalation Handoff
**Trigger**: Any ESC trigger fires and escalation is confirmed. Dynamic: `{escalation_reason}` and `{queue_name}` are injected.
```
I understand this requires specialized attention. I'm connecting you 
now with our {queue_name} team who can best assist you.

I've shared your complete conversation history with them so you 
won't need to repeat any details. Your estimated wait time is 
{estimated_wait_time}.

Thank you for your patience. 🙏
```

### PT-08: Complaint Acknowledgment
**Trigger**: Intent `CMP-001` (File Complaint) detected.
```
I'm sorry to hear about this experience. Your concern is important 
to us, and I want to make sure it's resolved properly.

I've logged your complaint with reference number {complaint_id}. 
Here's what happens next:
1. Our team will review your case within 24 hours.
2. You'll receive an update via SMS to your registered number.
3. If unresolved within 7 days, it will be auto-escalated to our 
   Principal Nodal Officer.

Is there anything else I can help you with?
```

### PT-09: Out-of-Scope Deflection
**Trigger**: Intent classified as `out_of_scope` with high confidence.
```
I appreciate your question! Unfortunately, that falls outside my area 
of expertise as a banking assistant. I'm best equipped to help with 
account inquiries, transactions, cards, loans, and NexBank products.

Is there a banking-related topic I can help you with?
```

### PT-10: Session Timeout Warning
**Trigger**: 4 minutes of user inactivity detected (1 minute before the 5-minute `session_timeout_minutes` config).
```
Just checking in — are you still there? 🕐 

Your session will close automatically in 1 minute for security reasons. 
If you need more time, just send any message and I'll be here!
```

### PT-11: After-Hours Notice
**Trigger**: P1/P2 escalation attempted outside business hours (9 PM – 8 AM IST).
```
Our specialist team is currently unavailable (business hours: 
8 AM – 9 PM IST). Here's what I can do for you:

📞 Schedule a callback for the next business day.
📝 Create a support ticket so our team has your details ready.

Which option would you prefer?
```

### PT-12: Crisis / Self-Harm Response
**Trigger**: `ESC-011` — Customer mentions self-harm, threats, or emergency. **This template is IMMUTABLE and cannot be modified by the learning pipeline.**
```
I hear you, and I want you to know that your well-being is the most 
important thing right now.

I'm immediately connecting you with a trained support specialist 
who can help. Please stay on the line.

If you or someone you know is in immediate danger, please call 
emergency services at 112 or the KIRAN Mental Health Helpline 
at 1800-599-0019 (toll-free, 24/7).
```

### PT-13: Fraud Report Intake
**Trigger**: `SEC-002` intent or `ESC-001` escalation trigger.
```
I'm taking this very seriously. To protect your account immediately, 
I need a few details:

1. Which account or card is affected? (Last 4 digits only)
2. When did you first notice the suspicious activity?
3. Have you shared your OTP, PIN, or password with anyone recently?

⚠️ For your safety, I've flagged your account for enhanced monitoring. 
Our Fraud Investigation team has been alerted and will contact you 
within {sla_time}.
```

### PT-14: Product Comparison
**Trigger**: User asks to compare 2+ NexBank products. Dynamic: `{product_table}` is generated from KB chunks.
```
Great question! Here's a side-by-side comparison based on the products 
you mentioned:

{product_table}

Would you like more details on any specific feature, or would you like 
me to help you with the application process?

---
📌 *This information is factual and does not constitute a recommendation. 
Please consult a certified financial advisor for personalized guidance.*
```

### PT-15: Loan EMI Calculation
**Trigger**: `LN-*` intent with `amount` and `tenure` entities successfully extracted.
```
Based on the details you provided:

💰 Loan Amount: ₹{amount}
📅 Tenure: {tenure} months
📊 Interest Rate: {rate}% p.a. (current NexBank rate)

Your estimated monthly EMI would be: ₹{emi_amount}

Total interest payable: ₹{total_interest}
Total amount payable: ₹{total_payable}

Would you like to explore eligibility or start an application?

---
📌 *This is an indicative calculation. Actual EMI may vary based on 
credit assessment and applicable terms.*
```

### PT-16: Farewell
**Trigger**: User signals end of conversation, intent `GEN-004`.
```
Thank you for banking with NexBank! 😊 

If you need help in the future, I'm always here. Have a wonderful day!

[Session ends. Post-chat CSAT survey is triggered.]
```
