# Data Flow

## Flow 1: Authenticated Support Request

1. User authenticates through SSO/OIDC.
2. Application receives identity claims and maps groups to roles.
3. Authorization layer checks tenant, account, workflow, and role.
4. Support case is routed to the assistant only if the user is allowed to operate on that tenant and case.

Trust boundary: identity provider to application.

## Flow 2: Evidence Retrieval

1. Assistant receives case ID and workflow label.
2. Retrieval layer requests policy, account, billing, integration, or security evidence.
3. Retrieval checks tenant scope and role permission before returning evidence.
4. Evidence references, not full sensitive content, are passed into eval where possible.

Trust boundary: application to evidence systems.

## Flow 3: AI Draft or Route Decision

1. Assistant generates an allowed action: answer with policy, draft for review, or route to specialist.
2. Output includes evidence references, confidence, and review flag.
3. p52-style eval service scores the output against expected action, evidence, review policy, cost, and latency.
4. Non-pass cases are held for review before customer response.

Trust boundary: deterministic policy/eval layer around model output.

## Flow 4: Audit and Monitoring

1. Request metadata, evidence references, score, failure modes, and final disposition are sent to audit logs.
2. Metrics aggregate pass rate, review rate, missing-evidence rate, override rate, latency, and rollback triggers.
3. Product feedback export strips or redacts customer-identifying content unless explicitly approved.

Trust boundary: operational logs and analytics.

## Data Handling Table

| Data | Collected | Stored | Logged | Exported | Control |
| --- | --- | --- | --- | --- | --- |
| User identity | Yes | Session/audit reference | User ID, role | No | SSO/OIDC, RBAC |
| Tenant/account ID | Yes | Audit and metrics | Redacted/reference form | Aggregated only | Tenant scoping |
| Ticket text | Yes | Case system | Avoid full raw text by default | Redacted themes | Retention and access control |
| Evidence references | Yes | Audit and eval record | Yes | Aggregated product feedback | Source/version metadata |
| Secrets/tokens | No assistant access | Secrets manager only | Never | Never | Rotation and least privilege |
| Model output | Yes | Eval/audit record | Summary and score | Aggregated quality metrics | Review gates |
| Product feedback | Yes | Product feedback store | Theme and reason code | PM review | Redaction and taxonomy |

## Retention Decisions

- Audit records: retention decided by customer contract and security policy.
- Raw ticket content: minimize retention outside the source support system.
- Eval fixtures: use synthetic or redacted examples for regression.
- Product feedback: aggregate by theme, workflow, and segment unless raw examples are approved.

