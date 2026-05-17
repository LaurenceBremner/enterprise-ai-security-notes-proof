# Architecture Note

## System Boundary

Synthetic target: an enterprise support-workflow assistant that drafts or routes support cases. It evaluates outputs with the p52 scoring service and follows the p53 discovery-to-rollout plan.

This note does not implement infrastructure. It documents where enterprise controls would sit before any production pilot.

## Components

| Component | Responsibility | Security/Privacy Concern |
| --- | --- | --- |
| Customer identity provider | Authenticates users through SSO/OIDC | Token validation, group mapping, session lifetime |
| Application gateway | Receives support workflow requests | Rate limits, request tracing, tenant routing |
| Authorization layer | Enforces RBAC by role and tenant | Privilege escalation, stale group membership |
| Workflow assistant | Drafts answers or routes cases | Prompt/data leakage, unsupported actions |
| Eval service | Scores action, evidence, confidence, review requirement, cost, and latency | Eval data integrity, explainability, safe rollout decisions |
| Evidence retrieval layer | Reads policy, account, billing, security, and integration evidence | Over-broad retrieval, stale documents, cross-tenant access |
| Audit log sink | Stores who asked, what evidence was used, and what action was taken | Sensitive log content, retention, access control |
| Secrets manager | Stores API credentials and signing secrets | Secret exposure, rotation, least privilege |
| Monitoring and alerting | Tracks failures, review queues, latency, and rollout gates | Logging customer content, alert fatigue |

## Identity and Access

- Use SSO/OIDC for user authentication.
- Map IdP groups to application roles: support agent, support lead, customer success, product reviewer, engineering reviewer, security reviewer, admin.
- Enforce tenant and account scoping before retrieval.
- Treat group and role sync as a pilot risk; stale permissions can expose customer data or bypass review.
- Do not use "internal user" as a security boundary.

## RBAC Policy Sketch

| Role | Allowed | Not Allowed |
| --- | --- | --- |
| Support agent | View assigned cases, request draft, submit review outcome | Override security review gate |
| Support lead | View team cases and override reasons | Change security or billing policy |
| Customer success | View pilot exposure and named-account risk | Access unrelated tenant data |
| Product reviewer | View anonymized themes and product gaps | View raw sensitive ticket data by default |
| Engineering reviewer | View integration diagnostics and logs for assigned cases | Access billing or security approval notes unless required |
| Security reviewer | Review access-control cases and audit trail | Change model prompts without review |
| Admin | Manage roles, config, and rollout cohorts | Bypass audit logging |

## Audit Logging

Minimum audit fields:

- request ID
- tenant ID
- account ID or redacted account reference
- user ID and role
- case ID
- workflow
- allowed action
- evidence references
- model/prompt version
- eval score
- detected failure modes
- human-review outcome
- final disposition
- timestamp

Avoid logging raw secrets, full access tokens, payment details, or unnecessary ticket content. Where raw content is needed for review, restrict access and define retention.

## Customer Data Boundary

- Retrieval must be tenant-scoped before any assistant call.
- Policy and account evidence should carry version and source metadata.
- Product feedback exports should be aggregated or redacted by default.
- Evaluation examples used for regression should be synthetic, approved, or redacted.
- Customer content should not be used for model improvement unless policy, contract, and consent decisions allow it.

## Secrets

- Store provider credentials, webhook signing secrets, and API tokens in a secrets manager.
- Rotate secrets on schedule and after suspected exposure.
- Never include secrets in prompts, eval fixtures, logs, screenshots, or support notes.
- Ensure local development uses separate non-production credentials.

## Operational Controls

- Health checks should verify service readiness without leaking tenant data.
- Rollout cohorts should be explicit and reversible.
- Review gates should fail closed when evidence or authorization is missing.
- Alerts should separate security blockers from model-quality review cases.

