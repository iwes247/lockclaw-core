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
**Task:** Update `iwes247/LockClaw` to look and read like an OpenClaw-ecosystem landing page (NullClaw-style README), and ensure it routes users cleanly to `lockclaw-baseline`, `lockclaw-appliance`, and `lockclaw-core`.

**Pseudocode Logic (Architect’s Plan):**
1. **CREATE ASSET PATHS:** In `LockClaw`, create:
   - `docs/assets/`
   - Add placeholder file reference for the hero mark: `docs/assets/lockclaw-mark.png` (do not invent proprietary art; use existing LockClaw mark or a simple placeholder until replaced).
2. **REPLACE README HERO:** Rewrite the top of `LockClaw/README.md` into a centered “hero” block (NullClaw-style):
   - Logo (image)
   - Project name (`LockClaw`)
   - One-line tagline: “Fail-closed security seatbelt for local LLM Docker deployments.”
   - A compact nav row: Baseline • Appliance • Core • Issues
   - Badges row (ONLY if truthful/real for this repo; avoid fake CI badges)
3. **ADD START HERE CHOOSER:** Add a scannable “Start Here (Pick One)” table that maps intent to repo:
   - Baseline (default recommendation)
   - Appliance (host hardening)
   - Core (policies/contracts; dev-only)
4. **ADD QUICKSTART (2 MIN):** Provide a short Quickstart that routes to baseline (do not duplicate long install steps here):
   - “Go to baseline Quickstart”
   - “Validate success looks like”
   - “Add appliance only if you need host-level hardening”
5. **DEFINE BOUNDARIES:** Add “What LockClaw is” and “What LockClaw is NOT” sections:
   - Explicitly state baseline != host hardening
   - Avoid absolute “secure” claims; use scope/assumptions language
6. **ADD REPO FIT DIAGRAM:** Include a simple ASCII diagram showing landing -> baseline/appliance -> core relationship.
7. **REMOVE LEGACY CONFUSION:** Ensure landing README does NOT read like an archived/historical repo:
   - Remove any “archived for historical reference” language
   - Move legacy content (pre-split docs/snippets) to `docs/legacy/` and link to it once as “Legacy (pre-split)”
8. **LINK HYGIENE:** Ensure links are correct and consistent:
   - `https://github.com/iwes247/lockclaw-baseline`
   - `https://github.com/iwes247/lockclaw-appliance`
   - `https://github.com/iwes247/lockclaw-core`
   - Remove/avoid old `ghcr.io/iwes247/lockclaw:*` references unless intentionally maintained
9. **CONTRIBUTING POINTER:** Keep the landing repo minimal:
   - Add “Contributing: see `lockclaw-core/CONTRIBUTING.md`” (do not include full workflow here)

**Red-Teamer Constraints (Safety Rails):**
- **No Overpromises:** Do not claim “secure”; describe fail-closed posture + scope + assumptions.
- **Badges Must Be Real:** Only include badges that actually exist and reflect this repo accurately.
- **No Install Target Confusion:** Landing repo must not include runnable old stack instructions; it routes users to baseline/appliance.
- **No Proprietary Asset Copying:** Do not copy OpenClaw/NullClaw art; use LockClaw’s own mark or a neutral placeholder.
- **Docs-Only:** Do not change runtime code in baseline/appliance/core as part of this task; landing repo README/docs only.

**Verification Command:**
1. Open `https://github.com/iwes247/LockClaw` in a fresh browser session.
2. Confirm within 15 seconds:
   - What LockClaw is (tagline visible)
   - Where to start (chooser visible)
   - Where to go next (links to baseline/appliance/core)
3. Click each primary link (Baseline/Appliance/Core/Issues) and confirm no 404s.
4. Confirm there is no “archived” messaging and no legacy install steps on the landing README.

—

## Draft README Layout (Paste Target for `LockClaw/README.md`)
> Claude: Use this layout and adjust only what’s needed to keep it truthful (badges, status, and any links).

<div align=“center”>
  <img src=“docs/assets/lockclaw-mark.png” width=“96” height=“96” alt=“LockClaw logo” />
  <h1>LockClaw</h1>
  <p><b>Fail-closed security seatbelt for local LLM Docker deployments.</b></p>

  <p>
    <a href=“https://github.com/iwes247/lockclaw-baseline”>Baseline</a> •
    <a href=“https://github.com/iwes247/lockclaw-appliance”>Appliance</a> •
    <a href=“https://github.com/iwes247/lockclaw-core”>Core</a> •
    <a href=“https://github.com/iwes247/LockClaw/issues”>Issues</a>
  </p>

  <p>
    <!— Badges: ONLY keep badges that are real/true for this repo —>
    <img alt=“license” src=“https://img.shields.io/badge/license-MIT-informational” />
    <img alt=“status” src=“https://img.shields.io/badge/status-alpha-yellow” />
    <img alt=“posture” src=“https://img.shields.io/badge/posture-fail—closed-blue” />
  </p>

  <p>
    <sub>
      Default-deny mindset • Minimal exposed surface • Loopback-first patterns • Clear scope boundaries
    </sub>
  </p>
</div>

—

## Start Here (Pick One)

| If you want… | Use… | What you get |
|—|—|—|
| Safe-by-default container runtime posture | **lockclaw-baseline** | Docker hardening defaults, loopback-first, minimal permissions |
| Host hardening (VPS/bare metal) | **lockclaw-appliance** | Firewall/SSH/auditd/fail2ban/AIDE-style host controls |
| Shared policies + contracts | **lockclaw-core** | Policy files, audits, versioned interfaces (dev-oriented) |

**Most users should start with:** `lockclaw-baseline`

—

## Quickstart (2 minutes)

1) Go to `lockclaw-baseline` and follow the Quickstart  
2) Confirm “success looks like” matches your environment  
3) Only add `lockclaw-appliance` if you need host-level hardening

Baseline Quickstart: https://github.com/iwes247/lockclaw-baseline

—

## What LockClaw is

LockClaw is a modular security baseline that helps you run local LLM stacks with fail-closed defaults and clear scope boundaries.

- **Baseline:** container/runtime guardrails  
- **Appliance:** host hardening controls  
- **Core:** shared policies, audits, and versioned interfaces  

## What LockClaw is NOT

- Not a replacement for host security hygiene  
- Not “secure by magic”  
- Not a vulnerability scanner product  
- Not a one-click internet-exposed deployment guide  

—

## How the repos fit together

LockClaw (landing)
  ├─ lockclaw-baseline  (container posture)
  │    └─ vendors lockclaw-core (policies + audits)
  └─ lockclaw-appliance (host posture)
       └─ consumes lockclaw-core conventions (where applicable)

—

## Why this exists

Many “local LLM” guides accidentally normalize unsafe defaults:
- published ports
- broad container permissions
- unclear boundaries between container vs host responsibility

LockClaw makes the safe path the easy path.

—

## Contributing

Contributing workflow (spec-first) lives in `lockclaw-core/CONTRIBUTING.md`.

—

## Legacy (pre-split)

Historical notes and the original pre-split README live under `docs/legacy/`.

## History

HISTORY_START
> ✅ FINISHED: 20260226 — Centralized contributing workflow in `lockclaw-core/CONTRIBUTING.md`, removed duplicated “vibe-sync” blocks from core/appliance READMEs, and added short pointer + fail-closed reminder in baseline/appliance READMEs.
> ✅ FINISHED: 20260223 — LockClaw v1 enforcement + cleanup pass completed across core, baseline, appliance. Added core fail-closed pre-flight and hobby/builder mode policies, removed duplicate baseline preflight, enforced read-only root + /data-only writable posture in baseline compose/entrypoint, aligned baseline/appliance docs, and synced vendored core policy artifacts.
> ✅ FINISHED: 20260221 — Repo clarity pass: added Start Here decision tree, ASCII architecture diagram, Success criteria, Baseline NOT host hardening warning, Core not standalone warning, Stability Contract, Versioning and Compatibility across all 3 repos.
HISTORY_END
