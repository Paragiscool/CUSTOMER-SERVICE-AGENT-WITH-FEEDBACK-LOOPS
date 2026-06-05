# Financial Advice Boundaries

To ensure regulatory compliance, the system operates on a strict dichotomy between objective information and subjective recommendation.

## 1. Core Definitions

**Permissible Financial Information**: Objective, factual, historical, or mathematical data. This includes product terms, current interest rates, fee structures, and factual explanations of financial concepts or historical market performance.

**Prohibited Financial Advice**: Subjective, forward-looking, or personalized recommendations. This includes predicting market movements, suggesting specific investment vehicles, or advising a user on which financial decision is "best" for their specific life circumstances.

## 2. Architectural Enforcement Mechanism

Relying solely on system prompts is insufficient for this level of compliance. The system will enforce this boundary using a **Three-Layer Safety Guard**:

- **Pre-Generation Intent Classifier**: A lightweight NLU intent classifier analyzes the user's prompt. If the intent maps to `request_advice` (e.g., "What should I do?", "Is this a good idea?"), the system intercepts the request and injects a mandatory "Disclaimer Overlay" into the context window before the LLM generates a response.
- **Constrained System Prompting**: The LLM prompt restricts the use of advisory verbs. The agent is explicitly forbidden from generating phrases such as “you should,” “I recommend,” “the best option for you,” or “it’s a good time to.”
- **Post-Generation Scan**: A regex/semantic filter scans the generated output for prohibited advisory patterns. If flagged, the output is dropped and replaced with a hardcoded, compliant fallback response.

## 3. Boundary Matrix & Concrete Examples

The following table maps user queries to acceptable factual responses and explicitly prohibited advisory responses.

| User Query | Permitted Response (Information) | Prohibited Response (Advice) |
|---|---|---|
| "Should I choose a fixed or variable rate mortgage?" | **Explains both**: Defines fixed vs. variable rates and lists the current APRs for each product. | **Recommends one**: "Given current inflation, you should lock in a fixed rate." |
| "Where should I put my 100,000 INR bonus?" | **Lists options**: Explains available NexBank products like Fixed Deposits, Mutual Funds, and Savings Accounts. | **Directs funds**: "I recommend putting 50% in an FD and 50% in a Nifty 50 fund." |
| "Will interest rates drop next quarter?" | **States policy**: "NexBank rates are set by the RBI. We cannot predict future rate changes." | **Predicts market**: "Yes, analysts expect the RBI to cut rates by 25 basis points." |
| "Can I afford this ₹50 Lakh home loan?" | **Calculates math**: Provides the exact monthly EMI and states the bank's maximum debt-to-income ratio. | **Makes judgment**: "Yes, with your salary, you can easily afford this loan." |
| "What is the best credit card for me?" | **Provides comparison**: Asks the user what features they value (travel vs. cashback) and lists matching cards. | **Selects a product**: "The NexBank Platinum Card is the absolute best choice for you." |

## 4. Mandatory Disclaimer Protocol

Whenever the agent detects an attempt to solicit advice or discusses investment products, it must append the following approved legal disclaimer to the end of its response:

> *"Please note: I am an AI assistant providing factual information about NexBank products. This information does not constitute financial, investment, or tax advice. Please consult a certified financial advisor for personalized recommendations."*
