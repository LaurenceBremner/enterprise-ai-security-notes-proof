# Methodology

## Motivation

I wanted this repo to show deployment-constraint thinking for enterprise AI work: what has to be controlled before an assistant can touch customer workflows.

## What I Tried

I used a synthetic support-workflow assistant and mapped the controls around it instead of pretending to build a complete production security platform.

## Method

1. Define the workflow and the data it needs.
2. Mark trust boundaries around identity, retrieval, model draft, action, and audit.
3. List failure modes that would block a pilot.
4. Map each failure to a control and verification question.
5. Keep caveats explicit where the artifact stops short of production implementation.

## Worked Example

A user asks the assistant to draft a customer response. Identity and tenant claims constrain retrieval, evidence is filtered before it reaches the model, the model draft is checked by an eval gate, and high-risk actions require human approval plus an audit record.

## Failure Modes

- Cross-tenant evidence leak.
- Prompt bypass of RBAC or connector filters.
- Unapproved send action.
- Missing audit trail for a blocked or sent response.
- Retention mismatch between logs, prompts, and source systems.

## Tradeoffs

This is notes-first and intentionally small. It is stronger for review conversation than for proving implementation depth.

## Limitations

The repo does not implement SSO, secrets management, private networking, Kubernetes, compliance certification, production audit storage, or tenant isolation.

## What I Would Change Next

Pair the notes with a runnable toy service that enforces tenant-scoped retrieval, audit events, and policy-based action gates.
