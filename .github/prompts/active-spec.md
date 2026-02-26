# LockClaw Core — Active Spec

> **This file is the phone-to-VSCode bridge.**
> Edit from your phone (via GPT) → push → pull in VS Code → Copilot reads and executes.

## Project summary

lockclaw-core contains shared policy definitions, audit scripts, and port
allowlists consumed by lockclaw-baseline and lockclaw-appliance. It is
vendored into each consuming repo at `lockclaw-core/`.

## Contents

```
audit/audit.sh         ← policy file validation
audit/port-check.sh    ← allowlist-driven port audit (hard-fail)
audit/pre-flight.sh    ← fail-closed startup posture enforcement
policies/ports/        ← per-profile port allowlists (JSON)
policies/modes/        ← hobby/builder runtime policy modes
policies/ssh-*         ← required SSH posture values
policies/sysctl-*      ← required sysctl values (appliance only)
scanner/security-scan.sh ← AIDE + rkhunter + Lynis wrapper
docs/                  ← threat model template
```

## Current Task

## Current Task
**Task:** Professionalize and centralize the contributing workflow: deduplicate the current “vibe-sync workflow” block, relocate it to `lockclaw-core`, and replace it everywhere else with a short pointer.

**Pseudocode Logic (Architect’s Plan):**
1. **FIND:** Locate the existing “Contributing — vibe-sync workflow” block across:
   - `LockClaw-baseline`
   - `lockclaw-appliance`
   - `lockclaw-core`
2. **EXTRACT:** Create a single source of truth doc in `lockclaw-core`:
   - Prefer `CONTRIBUTING.md`
   - (Alternative: `docs/WORKFLOW.md` if you want separate docs)
3. **REWRITE (Tone Fix):** Replace “vibe” language with security/professional language:
   - Rename references like `vibe-sync` -> `spec-sync` (or keep script names but describe them neutrally)
   - Remove phrases like “Copilot executes”
   - Emphasize auditability, validation, and review
4. **REPLACE (Baseline + Appliance):** Remove the full workflow block from baseline/appliance READMEs and replace it with a 1–2 line pointer:
   - “Contributing workflow: see `lockclaw-core/CONTRIBUTING.md`”
   - Changes must be reproducible and must not weaken fail-closed defaults
5. **VERIFY:** Confirm:
   - Only one canonical workflow description exists (in core)
   - All links resolve (no 404s)
   - Baseline/appliance READMEs remain user-focused (what it is / isn’t / how to use)

**Red-Teamer Constraints (Safety Rails):**
- **Docs-only:** No runtime/behavior changes, no Dockerfile changes, no security gate changes.
- **User-first READMEs:** Baseline/appliance README must stay product-focused (usage + scope + safety boundaries).
- **No Overpromises:** Do not claim AI outputs are correct; require validation and review.
- **No Internal-only Paths:** The workflow doc must not reference scripts/paths that don’t exist in the repo.

**Verification Command:**
1. Open `lockclaw-core/CONTRIBUTING.md` and confirm it contains the complete workflow text.
2. Open baseline + appliance READMEs and confirm the full block is removed and replaced with a pointer link to core.
3. Click the pointer link(s) and confirm they resolve.

—

### Contributing workflow (spec-first)
We use a spec-first workflow to keep changes auditable and reduce ambiguity.

1. Update the task spec in `.github/prompts/active-spec.md`.
2. Pull the spec at your workstation using the sync script.
3. Implement changes (AI assistance allowed, but optional).
4. Validate locally (tests + security checks).
5. Open a PR for review, then archive the completed spec.

## History

HISTORY_START
> ✅ FINISHED: 20260223 — LockClaw v1 enforcement + cleanup pass completed across core, baseline, appliance. Added core fail-closed pre-flight and hobby/builder mode policies, removed duplicate baseline preflight, enforced read-only root + /data-only writable posture in baseline compose/entrypoint, aligned baseline/appliance docs, and synced vendored core policy artifacts.
> ✅ FINISHED: 20260221 — Repo clarity pass: added Start Here decision tree, ASCII architecture diagram, Success criteria, Baseline NOT host hardening warning, Core not standalone warning, Stability Contract, Versioning and Compatibility across all 3 repos.
HISTORY_END
