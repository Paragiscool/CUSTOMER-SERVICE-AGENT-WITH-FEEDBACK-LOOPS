# Dashboard Wireframe Specifications

The NexBank Agentic AI requires three distinct dashboards to serve different operational cadences and stakeholder audiences.

## 1. Real-Time Operations Dashboard

**Audience**: L1 Support Ops, On-Call MLOps Engineers
**Refresh Rate**: Every 5 seconds
**Purpose**: Live system health and active threat monitoring.

### Layout

```
┌─────────────────────────────────────────────────────────────────────┐
│                    REAL-TIME OPERATIONS DASHBOARD                    │
├───────────────┬───────────────┬───────────────┬─────────────────────┤
│  Active       │  Rolling 1hr  │  Rolling 1hr  │  Rolling 1hr        │
│  Conversations│  CSAT         │  Containment  │  Escalation Rate    │
│  [1,247]      │  [4.6 / 5.0]  │  [73.2%]      │  [26.8%]            │
├───────────────┴───────────────┴───────────────┴─────────────────────┤
│                                                                     │
│  LIVE CONVERSATION VOLUME (Streaming Time-Series Chart)             │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
│  [Conversations per minute over the last 60 minutes]                │
│                                                                     │
├─────────────────────────────────┬───────────────────────────────────┤
│  GUARDRAIL TRIGGER HEAT MAP    │  ACTIVE ESCALATIONS               │
│                                │                                   │
│  Advice Boundary: ████ (42)    │  P0 (Fraud):     2 [IN PROGRESS]  │
│  PII Scrubbing:   ██ (18)     │  P0 (Security):  0                │
│  Injection Block: █ (7)       │  P1 (Retention): 4 [QUEUED]       │
│  Token Obfusc.:   ░ (2)      │  P2 (General):   11               │
│                                │                                   │
├─────────────────────────────────┴───────────────────────────────────┤
│  🚨 ANOMALY ALERT PANEL (Streaming Log)                            │
│                                                                     │
│  [12:04:32] WARNING: Fraud report spike (+300% vs baseline) — ESC-001│
│  [12:01:15] INFO: Escalation queue P1 at 85% capacity              │
│  [11:58:44] INFO: KB chunk "FD_rates_2026" retrieved 200+ times/hr │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. Daily Performance Dashboard

**Audience**: Team Leads, Product Managers
**Refresh Rate**: Hourly aggregation, reviewed once daily
**Purpose**: Identify trends, review flagged interactions, and prioritize improvements.

### Layout

```
┌─────────────────────────────────────────────────────────────────────┐
│                    DAILY PERFORMANCE DASHBOARD                      │
├───────────────┬───────────────┬───────────────┬─────────────────────┤
│  Total        │  Avg CSAT     │  FCR          │  Avg Handle         │
│  Sessions     │  (24hr)       │  (24hr)       │  Time (24hr)        │
│  [18,412]     │  [4.52]       │  [71.3%]      │  [6.2 min]          │
├───────────────┴───────────────┴───────────────┴─────────────────────┤
│                                                                     │
│  24-HOUR TREND LINES (Leading + Lagging Indicators)                 │
│  ── CSAT  ── Intent Accuracy  ── Containment Rate  ── Escalation   │
│  [Multi-line time-series chart with hourly data points]             │
│                                                                     │
├─────────────────────────────────┬───────────────────────────────────┤
│  CONVERSATION FUNNEL            │  TOP 10 UNRESOLVED INTENTS        │
│                                │                                   │
│  Started:     18,412 (100%)    │  1. TXN-003 (Dispute) — 142       │
│  Engaged:     17,890 (97.2%)   │  2. ACC-005 (Closure) — 98        │
│  Resolved:    13,124 (71.3%)   │  3. CRD-002 (Limit) — 87         │
│  Satisfied:   11,841 (64.3%)   │  4. SEC-001 (Fraud) — 76         │
│                                │  5. LN-001 (Home Loan) — 63       │
│                                │                                   │
├─────────────────────────────────┴───────────────────────────────────┤
│  SUPERVISOR CORRECTION SUMMARY                                      │
│                                                                     │
│  Intent Misclassification: 23  |  Entity Extraction Fail: 12       │
│  Tone / Empathy Failure:   8   |  Hallucination / KB Miss: 4       │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3. Weekly Strategic Dashboard

**Audience**: VP of Customer Experience, Head of AI/ML, Chief Risk Officer
**Refresh Rate**: Weekly aggregation, reviewed in Monday leadership meetings
**Purpose**: Strategic decision-making, competitive benchmarking, and investment prioritization.

### Layout

```
┌─────────────────────────────────────────────────────────────────────┐
│                    WEEKLY STRATEGIC DASHBOARD                       │
├───────────────┬───────────────┬───────────────┬─────────────────────┤
│  WoW CSAT     │  WoW FCR      │  WoW          │  Model Version      │
│  Change       │  Change       │  Containment  │  (Current)          │
│  [+0.12 ▲]    │  [+1.3% ▲]    │  [-0.5% ▼]    │  [v2.3-LoRA-r16]   │
├───────────────┴───────────────┴───────────────┴─────────────────────┤
│                                                                     │
│  WEEK-OVER-WEEK KPI IMPROVEMENT TRENDS                              │
│  [Bar chart comparing current week vs previous 4 weeks for all KPIs]│
│                                                                     │
├─────────────────────────────────┬───────────────────────────────────┤
│  A/B TEST RESULTS              │  KB GAP ANALYSIS                  │
│                                │                                   │
│  Experiment: Empathy-v2 Prompt │  Unaddressed Queries: 847 (4.6%)  │
│  Status: ✅ Winner (p<0.05)    │  Top Gap: "UPI refund timeline"   │
│  CSAT Impact: +0.18            │  Recommended: Add 3 new KB chunks │
│  Deploying to 100% on Wed     │                                   │
│                                │                                   │
├─────────────────────────────────┴───────────────────────────────────┤
│  LEARNING PIPELINE METRICS                                          │
│                                                                     │
│  Training Data Volume: 12,450 labeled pairs (+8% WoW)              │
│  Model Version History: v2.1 → v2.2 → v2.3 (current)              │
│  Safety Test Pass Rate: 200/200 (100%) ✅                          │
│  Pending Experiments: 2 (Slot-filling-v3, RAG-rerank-tuning)       │
└─────────────────────────────────────────────────────────────────────┘
```
