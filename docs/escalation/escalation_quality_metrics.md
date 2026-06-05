# Escalation Quality Metrics

To ensure the escalation system is effective and not simply "throwing problems over the wall" to human agents, the following metrics track the quality and efficiency of every handoff.

## 1. Escalation Efficiency Metrics

| Metric | Definition | Target | Frequency |
|---|---|---|---|
| **Time-to-Pickup** | Time between escalation trigger and human agent accepting the session. | P0: <2 min, P1: <5 min, P2: <10 min | Real-time |
| **Handoff Completeness Score** | Percentage of the 8-element handoff payload successfully transferred (e.g., was the sentiment trajectory included?). | 100% | Daily |
| **Customer Repeat Rate** | Percentage of customers who had to repeat information already captured by the AI to the human agent. | <5% | Weekly |
| **Post-Escalation CSAT** | CSAT score collected after the human agent resolves the escalated conversation. Compared against the baseline CSAT for non-escalated sessions. | ≥4.0 / 5.0 | Weekly |

## 2. Escalation Appropriateness Metrics

| Metric | Definition | Target | Frequency |
|---|---|---|---|
| **Unnecessary Escalation Rate** | Percentage of escalated conversations where the human agent determines the AI could have resolved the issue. | <10% | Weekly |
| **Missed Escalation Rate** | Percentage of non-escalated conversations where a post-hoc analysis determines a human should have been involved (e.g., the customer later filed a formal complaint). | <2% | Weekly |
| **De-Escalation Success Rate** | For P2 triggers, the percentage of de-escalation attempts that successfully retained the conversation in the AI channel. | >30% | Weekly |

## 3. Operational Load Metrics

| Metric | Definition | Target | Frequency |
|---|---|---|---|
| **Escalation Volume by Queue** | Number of escalations per target queue per hour. Used for staffing and capacity planning. | Within forecast ±15% | Hourly |
| **Queue Overflow Rate** | Percentage of escalations that triggered the overflow handling protocol due to exceeding 2x SLA wait time. | <5% | Daily |
| **After-Hours Escalation Volume** | Number of P0/P1 escalations outside business hours. Determines on-call staffing needs. | Trending downward | Weekly |
