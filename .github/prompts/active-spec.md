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

[READY FOR NEXT VIBE]

## History

HISTORY_START
> ✅ FINISHED: 20260223 — LockClaw v1 enforcement + cleanup pass completed across core, baseline, appliance. Added core fail-closed pre-flight and hobby/builder mode policies, removed duplicate baseline preflight, enforced read-only root + /data-only writable posture in baseline compose/entrypoint, aligned baseline/appliance docs, and synced vendored core policy artifacts.
> ✅ FINISHED: 20260221 — Repo clarity pass: added Start Here decision tree, ASCII architecture diagram, Success criteria, Baseline NOT host hardening warning, Core not standalone warning, Stability Contract, Versioning and Compatibility across all 3 repos.
HISTORY_END
