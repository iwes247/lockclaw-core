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

[READY FOR NEXT VIBE]

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
