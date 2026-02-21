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
policies/ports/        ← per-profile port allowlists (JSON)
policies/ssh-*         ← required SSH posture values
policies/sysctl-*      ← required sysctl values (appliance only)
scanner/security-scan.sh ← AIDE + rkhunter + Lynis wrapper
docs/                  ← threat model template
```

## Current Task
**Task:** Finish and publish the “Repo Clarity Pass” across the 3 repos so a newcomer can pick the right repo in <15 seconds.

### Scope (Repos)
- `LockClaw-baseline`
- `lockclaw-appliance`  (make sure EVERYTHING says **appliance**, not “application”)
- `LockClaw-core` (source of truth for the spec lives here)

**NOTE:** The current published READMEs do not yet show the shared “Start Here (Pick One)” chooser / diagram / success checklist in baseline + appliance, and `lockclaw-core` does not clearly expose a “Stability Contract” section. Also, `lockclaw-core/.github/prompts/active-spec.md` appears stale (“READY FOR NEXT VIBE”) compared to what’s actually published.

—

## Pseudocode Logic (Architect’s Plan)
1. **SCAN:** Open each repo README and capture the first-screen content (top ~40 lines). Note:
   - Tagline clarity
   - Who it’s for
   - What it does NOT do
   - Cross-links correctness
   - Any “application vs appliance” naming drift
2. **NORMALIZE:** Standardize naming everywhere:
   - Brand: “LockClaw”
   - Repo names: `LockClaw-baseline`, `lockclaw-appliance`, `LockClaw-core`
   - Eliminate “application” references entirely
3. **INSERT (ALL 3 READMEs):** Add the same three sections (identical wording) near the top:
   - **Start Here (Pick One):** A short decision tree mapping use-cases to the correct repo
   - **How the repos fit together:** Include a small ASCII diagram showing baseline/appliance consuming core
   - **Success looks like:** 4–6 observable outcomes (no commands)
4. **INSERT (CORE ONLY):** Add two sections to `LockClaw-core` README:
   - **Stability Contract:** Explicit stable surfaces (paths/formats/schemas) vs internal/unstable
   - **Versioning & Compatibility:** Tag strategy (e.g., `core-vX.Y.Z`), what counts as breaking, and how baseline/appliance pin + upgrade
5. **REINFORCE (BASELINE ONLY):** Add a prominent warning box in the first screen:
   - “Baseline is NOT host hardening”
   - “Assumes host is already reasonably secured”
   - “No published ports by default”
6. **METADATA:** Add/update GitHub “About” descriptions + topics for all three repos (especially `LockClaw-core`)
7. **UPDATE SPEC:** Refresh `lockclaw-core/.github/prompts/active-spec.md` so it describes THIS task (not “READY FOR NEXT VIBE”)
8. **VALIDATE:** Re-open each README and confirm a newcomer can answer quickly:
   - Which repo do I use?
   - What does it do?
   - What does it NOT do?
   - Where do I go next? (links correct, no 404s)

—

## Red-Teamer Constraints (Safety Rails)
- **No Security Claim Drift:** Do NOT imply baseline secures the host (firewall/ssh/audit are appliance scope).
- **No Behavior Changes:** Documentation + metadata only; do not change defaults, privileges, entrypoints, or capabilities.
- **No Core-as-Install Confusion:** Core must clearly read as a shared library/policies repo; most users should not clone it directly.
- **IPv6 Wording:** Avoid misleading statements; phrase as “bind to localhost/loopback” without implying firewall guarantees.
- **Link Integrity:** All cross-links must resolve and point to correct repo names; eliminate any `LockClaw-application` references.

—

## Acceptance Criteria
- Every README has: **Start Here (Pick One)** + **Repo Fit Diagram** + **Success looks like** in the top portion.
- Baseline README includes a **blunt warning box** that it is not host hardening.
- Core README includes **Stability Contract** + **Versioning & Compatibility** sections.
- All links resolve (no 404s), and naming is consistent (“appliance” everywhere).
- Core spec file reflects current work and is not stale.

—

## Verification Command
Manual verification (fast):
- Open each README in GitHub web UI:
  - Confirm the chooser exists in the first screenful
  - Confirm cross-links go to correct repos
  - Confirm baseline warning box is prominent
  - Confirm core has Stability Contract + Versioning sections

Optional local check (if needed):
- `git grep -i “application” -n` in each repo and confirm zero matches (except historical changelog notes if any).

—

## Red-Teamer Constraints (Safety Rails)
- **No Security Claim Drift:** Do NOT imply baseline secures the host (firewall/ssh/audit are appliance scope).
- **No Behavior Changes:** Documentation + metadata only; do not change defaults, privileges, entrypoints, or capabilities.
- **No Core-as-Install Confusion:** Core must clearly read as a shared library/policies repo; most users should not clone it directly.
- **IPv6 Wording:** Avoid misleading statements; phrase as “bind to localhost/loopback” without implying firewall guarantees.
- **Link Integrity:** All cross-links must resolve and point to correct repo names; eliminate any `LockClaw-application` references.

—

## Acceptance Criteria
- Every README has: **Start Here (Pick One)** + **Repo Fit Diagram** + **Success looks like** in the top portion.
- Baseline README includes a **blunt warning box** that it is not host hardening.
- Core README includes **Stability Contract** + **Versioning & Compatibility** sections.
- All links resolve (no 404s), and naming is consistent (“appliance” everywhere).
- Core spec file reflects current work and is not stale.

—

## Verification Command
Manual verification (fast):
- Open each README in GitHub web UI:
  - Confirm the chooser exists in the first screenful
  - Confirm cross-links go to correct repos
  - Confirm baseline warning box is prominent
  - Confirm core has Stability Contract + Versioning sections

Optional local check (if needed):
- `git grep -i “application” -n` in each repo and confirm zero matches (except historical changelog notes if any).

## History

HISTORY_START
> ✅ FINISHED: 20260221 — Repo clarity pass: added Start Here decision tree, ASCII architecture diagram, Success criteria, Baseline NOT host hardening warning, Core not standalone warning, Stability Contract, Versioning and Compatibility across all 3 repos.
HISTORY_END
