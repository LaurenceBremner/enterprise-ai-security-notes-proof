# Enterprise AI Security Notes

Compact architecture and security notes for a synthetic AI workflow assistant entering a controlled pilot.

Live page: https://laurencebremner.github.io/enterprise-ai-security-notes-proof/

## What It Shows

- Trust boundaries for identity, retrieval, model draft, action, and audit.
- Data-flow notes for tenant-scoped evidence and pre-response gates.
- A risk register covering cross-tenant leaks, unapproved actions, audit gaps, and retention mismatches.
- Explicit caveats about what is not implemented.

## Method

The method is documented in [`METHODOLOGY.md`](METHODOLOGY.md). In short: start from a concrete workflow, draw trust boundaries, list pilot-blocking failure modes, map controls to verification questions, and keep caveats explicit.

## Artifact Set

- `docs/index.html`: public architecture/security review page.
- `architecture_note.md`: integration boundaries, identity, RBAC, audit, data, secrets, and operational controls.
- `data_flow.md`: what enters the system, where it is processed, logged, retained, exported, or blocked.
- `threat_model.md`: assets, actors, entry points, trust boundaries, risks, mitigations, and open decisions.
- `security_review_checklist.md`: lightweight pre-pilot review checklist.
- `out_of_scope.md`: explicit production limits.
- `METHODOLOGY.md`: rationale, method, worked example, failure modes, and next steps.

## Boundary

This is a notes-only synthetic artifact. It does not implement SSO, private networking, Kubernetes, secrets management, production audit storage, tenant isolation, or compliance certification.
