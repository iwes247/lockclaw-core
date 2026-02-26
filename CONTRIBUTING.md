# Contributing

LockClaw uses a spec-first workflow so changes remain auditable, reproducible, and reviewable.

## Workflow

1. Update the active task in `.github/prompts/active-spec.md`.
2. Pull the latest repo state at your workstation.
3. Implement the task (AI assistance is optional).
4. Validate locally before opening a PR:
   - run relevant tests and smoke checks
   - verify security posture checks still pass
   - confirm no change weakens fail-closed defaults
5. Open a PR for review with a clear summary of what changed and why.
6. After merge, reset/archive the completed spec in `.github/prompts/active-spec.md`.

## Guardrails

- Do not weaken deny-by-default and fail-closed behavior.
- Keep claims in docs accurate; avoid overpromising outcomes.
- Treat generated changes as drafts: review, test, and verify before merge.
- Keep changes minimal, reproducible, and scoped to the active spec.
