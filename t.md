5. Open Questions & Decisions Needed (Product / Analytics Review)

This section captures items identified during analysis and requires confirmation before final experiment configuration.

Q1. Company ID / GPID Exclusions

Observation: Some GPIDs / Company IDs are not currently present in the standard A/B exclusion list.

Question: Should these be explicitly added to this experiment’s exclusion list?

Recommendation: Yes — create a dedicated exclusion list scoped to this experiment to avoid unintended exposure and allow independent control.

Q2. Should This Experiment Use a New Config?

Observation: Existing preview and rollout configs are shared across multiple surfaces.

Question: Should this experiment use a new, isolated configuration?

Recommendation: Yes — create a new experiment config to allow independent ramping, rollback, and analytics isolation.

Q3. Traveler ID (TUID) Not Eligible When Preview Is Enabled

Observation: Current logic excludes TUID-based users when preview is enabled.

Question: How will analytics track such users, and is this restriction still required?

Ask from Analytics: Please confirm:

Whether excluding TUID users affects experiment attribution

Whether this rule can be removed without impacting data correctness

Q4. UITK Agency App Preview Enabled Config

Observation: There exists a UITK Agency App preview config that forces FNG enablement.

Question: Should experiment routing override this preview config, or should preview continue to take precedence for those users?

Recommendation: Experiment should take precedence to preserve clean cohorting, but confirmation required.

6. Testing & Overrides (Engineering / QA Use)
Forced Bucket Override for Validation

Requirement: Ability to force a user/session/company into a specific variant for testing.

Proposal:

Support override via:

Request header

Query param

Internal config toggle

This override should:

Bypass bucketing

Respect hard exclusion rules

Be disabled in production-facing flows

7. Monitoring & Rollback (Optional)

Metrics tracked: conversion rate, error rate, latency

Rollback: Disable experiment config to route 100% traffic to control
