# Threat Model

## Scope

Synthetic enterprise AI workflow assistant for support operations. The scope covers identity, access control, evidence retrieval, AI output evaluation, audit logging, secrets, customer data boundaries, product feedback exports, and rollout controls.

Out of scope: legal compliance certification, cloud network implementation, Kubernetes deployment, vendor procurement, penetration testing, and formal privacy counsel.

## Assets

- Customer ticket content.
- Tenant and account identifiers.
- Policy, billing, integration, and security evidence.
- User identity and role claims.
- Model prompts and outputs.
- Eval scores and failure modes.
- Audit logs and review outcomes.
- API keys, provider credentials, and webhook signing secrets.
- Product feedback themes and exports.

## Actors

- Support agent.
- Support lead.
- Customer success manager.
- Product manager.
- Engineering reviewer.
- Security reviewer.
- Application admin.
- External customer.
- Malicious or over-permissioned internal user.
- Third-party model or API provider.

## Entry Points

- Support request intake.
- SSO/OIDC callback.
- Evidence retrieval API.
- AI assistant request.
- Eval service request.
- Admin configuration panel.
- Audit log viewer.
- Product feedback export.
- Webhook or integration diagnostic endpoint.

## Trust Boundaries

- Identity provider to application.
- User role claims to authorization layer.
- Application to customer evidence systems.
- Application to third-party model provider.
- Model output to deterministic eval/rollout gate.
- Operational logs to analytics or audit stores.
- Product feedback exports to roadmap systems.

## Risks

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| Cross-tenant data exposure | Medium | High | Tenant scoping before retrieval, tenant ID on every query, audit checks, tests for denied access |
| Privilege escalation through stale IdP groups | Medium | High | Frequent group sync, short session lifetime, server-side role checks, admin review |
| Secrets exposed in prompts or logs | Low-medium | High | Secrets manager, prompt filters, logging denylist, redaction tests |
| AI output bypasses required review | Medium | High | Deterministic review policy, fail-closed gates, blocker alerts |
| Evidence hallucination | Medium | High | Require evidence references, source/version metadata, missing-evidence review path |
| Raw customer data over-logged | Medium | Medium-high | Minimize logs, redact ticket content, define retention, restrict log access |
| Product feedback leaks customer identity | Medium | Medium | Aggregate by theme/segment, redact examples, approval for raw quotes |
| Third-party model data use mismatch | Medium | High | Contract/policy decision before sending customer data, provider allowlist, opt-out path |
| Replay or spoofed integration event | Low-medium | Medium-high | Verify signatures, timestamps, nonce or idempotency key, rotate signing secrets |
| Rollout cohort too broad | Medium | Medium-high | Explicit cohort config, named-account exclusions, rollback switch |

## Mitigations

- Enforce SSO/OIDC authentication and server-side RBAC.
- Scope retrieval by tenant and account before prompt construction.
- Keep secrets out of prompts, eval fixtures, logs, and product feedback exports.
- Store minimal audit fields with redaction rules.
- Require evidence references and source versions for assistant outputs.
- Fail closed on missing authorization, missing evidence, or required review.
- Separate security, billing, and routine support workflows in rollout policy.
- Keep pilot cohorts explicit and reversible.
- Use synthetic or redacted eval cases for regression.
- Distinguish engineering controls from product/legal decisions.

## Verification

- Access-control tests for tenant and role mismatch.
- Redaction tests for logs and product feedback exports.
- Eval cases for required review, missing evidence, and action mismatch.
- Secret scanning for prompts, fixtures, logs, and screenshots.
- Audit-log completeness checks.
- Manual security review before adding workflows with billing, access, or regulated data.

## Open Decisions

- Which customer data, if any, can be sent to a third-party model provider.
- Raw ticket retention outside the source support system.
- Whether product feedback exports can include raw examples.
- Audit retention duration by customer contract.
- Named-account exclusion process for pilot cohorts.
- Who owns final approval for security-sensitive workflows.

