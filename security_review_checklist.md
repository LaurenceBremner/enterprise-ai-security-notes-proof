# Security Review Checklist

## Identity and Access

- SSO/OIDC configured for pilot users.
- Role mapping reviewed by support, success, engineering, and security owners.
- Server-side authorization enforced for tenant, account, workflow, and action.
- Admin actions are audited.
- Review gates fail closed on missing permissions.

## Data Boundaries

- Retrieval is tenant-scoped before any assistant call.
- Evidence sources include version and source metadata.
- Product feedback exports are aggregated or redacted by default.
- Synthetic or redacted cases are used for regression evals.
- Customer data use with third-party model providers has an explicit product/legal decision.

## Logging and Audit

- Logs avoid raw secrets, tokens, payment details, and unnecessary raw ticket content.
- Audit records include request ID, user/role, tenant/account reference, workflow, evidence references, eval score, failure modes, review outcome, and final disposition.
- Audit-log access is role-restricted.
- Retention period is defined before pilot.

## Secrets and Integrations

- Provider credentials and webhook signing secrets are stored in a secrets manager.
- Local development uses non-production credentials.
- Integration webhooks validate signatures and timestamps.
- Secrets are rotated after suspected exposure.
- No secrets appear in prompts, test fixtures, screenshots, or exported notes.

## Rollout Controls

- Pilot cohort is explicit and reversible.
- Named-account exclusions are supported.
- Security and billing workflows remain review-gated.
- Rollback triggers are documented.
- Product feedback does not hide blocked automation cases.

## Open Before Production

- Formal threat review by security owner.
- Data processing and model-provider policy review.
- Incident response owner.
- Support/operator training.
- Monitoring and alerting owner.

