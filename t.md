
light Shopping Next-Gen UI — A/A → A/B Experiment Setup (Agencia)
1. Purpose

To enable controlled rollout of the Next-Gen Flight Shopping UI using the experimentation platform, replacing the current preview-based enablement (~6% traffic) with a statistically valid A/A followed by A/B experiment.

This document defines:

Eligibility rules

Exclusion rules

A/A and A/B setup

Operational guardrails

2. Eligibility Criteria (Who Can Enter the Experiment)

A request is eligible for experiment evaluation only if all of the following are true:

Rule	Condition
E1	POS ∈ {US, CA}
E2	User Type ∈ {Agent, TC, Traveler}
E3	Channel = Online shopping
E4	Not part of another active flight shopping experiment
E5	Request originates from supported web surfaces only
3. Exclusion Criteria (Hard Blocks)

Requests will be force-routed to Legacy UI if any of the following are true:

Rule	Condition
X1	Company ID ∈ exclusion list (see Appendix A)
X2	Mobile app traffic
X3	Offline / agent-assisted flows
X4	Unsupported POS
X5	Feature parity gaps or regulatory constraints
4. Preview Mode vs Experimentation

Previously, Next-Gen UI exposure was controlled via manual preview enablement (~6% traffic), which limited:

Conversion analysis

Statistical confidence

Controlled cohort comparisons

This experiment replaces preview-driven exposure with deterministic experimentation while preserving preview for:

Internal validation

Debugging

Targeted demos

Preview logic will not override experiment bucketing.

5. A/A Test Setup (Validation Phase)

Before starting A/B, an A/A test will be run to validate:

Bucketing correctness

Routing determinism

Metric parity

Observability pipeline correctness

Configuration
Parameter	Value
Variants	A1 = Legacy UI, A2 = Legacy UI
Split	50 / 50
Stripe	companyId (fallback: userId)
Eligibility	Same as Section 2
Exclusions	Same as Section 3
Exit Criteria

No statistically significant difference in conversion, latency, or error rates

No routing inconsistencies observed

6. A/B Test Setup (Post A/A Validation)

Once A/A is validated, the experiment will transition to A/B:

Parameter	Value
Control	Legacy UI
Treatment	Next-Gen UI
Initial Split	95 / 5
Ramp Plan	95/5 → 75/25 → 50/50
Stripe	companyId (fallback: userId)
Eligibility	Same as Section 2
Exclusions	Same as Section 3
7. Operational Behavior

Bucketing happens before UI rendering.

Users remain sticky to their assigned variant.

Preview enablement does not override experiment routing.

Excluded requests are always routed to Legacy UI.

8. Monitoring & Rollback
Monitored Signals

Conversion rate

Error rate (5xx / UI failures)

Search latency (P95)

Rollback Conditions
Trigger	Action
Error rate regression	Force 100% to Control
Latency degradation	Pause experiment
Business KPI drop	Roll back

Rollback is executed by disabling the experiment flag in the experimentation UI.

9. Ownership
Role	Owner
Experiment Setup	Angad
Product Approval	
QA Validation	
On-call	Flight Shopping Platform
