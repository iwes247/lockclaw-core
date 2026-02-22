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
# LOCKCLAW v1 ENFORCEMENT + DEAD CODE CLEANUP (OUTBOUND ENFORCEMENT DEFERRED)

You are performing a surgical refactor across the LockClaw repos.

Repos in scope:
- lockclaw-core
- lockclaw-baseline
- lockclaw-appliance

Goal:
Ship LockClaw v1 as a fail-closed baseline for containerized AI runtimes.

v1 focus: prevent self-inflicted exposure from unsafe container configuration.
Outbound **enforcement** is explicitly deferred (baseline will log posture; appliance can enforce later).

DO NOT expand scope.
DO NOT add features.
DO NOT introduce new repos.
DO NOT introduce heavy dependencies.
DO NOT implement firewall-level networking.
DO NOT add dashboards or SaaS components.

This is an enforcement + cleanup pass.

—

# FROZEN IDENTITY

LockClaw enforces locked-down defaults for containerized AI runtimes.
It prevents accidental exposure caused by unsafe container configuration.

It does not inspect agent logic.
It does not replace host security controls.
It does not enforce outbound network restrictions in baseline v1 (deferred to appliance/advanced setups).

All code and README updates must align with this.

—

# PHASE 1 — REMOVE DEAD CODE (AGGRESSIVE BUT SAFE)

Across all repos:

1) Remove:
- Unused scripts and stale helpers not invoked by entrypoint/compose/CI
- Duplicate pre-flight logic (keep only one authoritative preflight in core)
- Hardcoded port allowlists in baseline scripts (must defer to core policy)
- Any experimental firewall logic not used in v1
- Any “Lab mode” references
- Any claims of URL logging or domain allowlists that are not implemented
- Any duplicated policy definitions

2) Ensure:
- lockclaw-core is the single source of truth for policy and preflight enforcement
- baseline contains wiring and defaults only
- appliance remains host-level hardening documentation + optional hard controls

If unsure whether code is needed:
Prefer delete.
Keep diffs minimal and revertible.

—

# PHASE 2 — POLICY CONSOLIDATION (lockclaw-core)

Create:
- policies/modes/hobby.json
- policies/modes/builder.json

Each must define:
- mode
- allowed_ports (explicit list OR reference a profile, but must be consistent)
- writable_paths ([“/data”])
- egress_policy (“allow” in v1 baseline; outbound enforcement deferred)
- egress_logging (“none” or “banner_only” in v1 baseline)
- notes (short strings used in logs)

Update:
- policies/ports/container.json
Ensure it matches v1 reality.
Remove ambiguity.
No conflicting allowlists.

Create or finalize:
- audit/pre-flight.sh

Pre-flight MUST (fail-closed):
- Ports: parse listening ports via `ss -ltn` and validate against allowlist
- Filesystem: verify root filesystem is read-only; verify only `/data` is writable
- Mounts: detect RW mounts outside `/data` and fail
- Privilege/caps: fail on dangerous capabilities (SYS_ADMIN, NET_ADMIN, SYS_PTRACE, DAC_OVERRIDE, etc.)
- Privileged: fail if container is effectively privileged (conservative checks)
- Startup posture: print a concise summary of enforced posture
- Exit non-zero on any violation with loud actionable messages

No soft warnings for violations.
Only warnings allowed are for non-enforced outbound posture (banner only).

—

# PHASE 3 — BASELINE ENFORCEMENT (lockclaw-baseline)

Update docker-compose.yml:

Must include:
- read_only: true
- writable volume only at /data
- tmpfs mounts for required runtime paths (e.g., /tmp, /run, /var/tmp)
- security_opt: [“no-new-privileges:true”]
- cap_drop: [“ALL”] (unless truly required; if required, document why)
- No published ports by default

Outbound policy:
- Do NOT implement outbound enforcement in baseline v1.
- DO log clearly on startup: “Outbound network is allowed; baseline v1 does not enforce egress restrictions.”

Update docker-entrypoint.sh:

- Default LOCKCLAW_MODE=hobby
- Load mode policy from lockclaw-core
- Print startup banner:
  Mode
  Allowed Ports
  Writable Paths
  Root FS: read-only
  Caps: dropped + no-new-privileges
  Outbound: allowed (not enforced in baseline v1)
- Execute core preflight before runtime
- Abort immediately on failure

Remove any local preflight duplication in baseline.
If scripts/pre-flight.sh exists, turn it into a stub that delegates to core OR delete it and update references.

—

# PHASE 4 — README ALIGNMENT (NO OVERCLAIMS)

Baseline README must include:

Top identity:
“LockClaw enforces locked-down defaults for containerized AI runtimes.”
“It prevents accidental exposure caused by unsafe container configuration.”

Sections (tight):
- What it does (v1 only): ports allowlist, read-only root, /data writable, preflight fail-closed, startup posture logging, Hobby/Builder modes
- What it is NOT: not a dashboard, not SaaS, not agent firewall, not prompt inspection, not host hardening
- Quickstart (60 seconds): default run in Hobby mode
- Threat model bullets: focus on accidental exposure (ports/mounts/privilege). Explicitly say outbound enforcement is not included in baseline v1.
- Modes: Hobby vs Builder explained plainly (Builder = fewer guardrails only where explicitly allowed; still uses preflight)

Remove:
- firewall claims
- tool-call enforcement claims
- prompt injection mitigation claims
- URL logging claims
- “deny outbound” claims in baseline

Appliance README:
- Clarify appliance is where host-level firewall/auditing/egress controls may live
- No overlapping claims

—

# TESTING REQUIREMENTS (MUST PASS)

1) Default compose up
Expected:
- Hobby mode banner
- Root FS read-only
- Only /data writable
- No published ports
- Preflight PASSED

2) Fail-fast tests (each must fail preflight + exit non-zero + actionable message):
- Add a published port mapping
- Add RW bind mount outside /data
- Add privileged / dangerous caps (or simulate via config)
- Start a listener on an unallowlisted port

3) Builder mode:
LOCKCLAW_MODE=builder
- Banner shows Builder
- Still enforces RO FS + mounts + caps + ports
- Logs outbound is allowed (not enforced)

—

# OUTPUT REQUIREMENTS

- One focused commit per repo (core, baseline, appliance)
- Minimal diffs
- No sweeping refactors
- Remove dead code as you go (prefer delete if unused)
- Keep names consistent: LOCKCLAW_MODE, /data, Hobby/Builder
- 
- # Add: Dead Code Deletion Plan (Reviewable)

Before deleting anything, generate a **Dead Code Deletion Plan** as a markdown list.

## Requirements
For each candidate file/dir to delete, include:
- **Path:** `relative/path`
- **Repo:** core | baseline | appliance
- **Why it’s dead:** (1 sentence)
- **Proof it’s unused:** one of:
  - “Not referenced by compose/entrypoint/CI” (and list what you searched)
  - “Superseded by core preflight/policy” (and name the replacement path)
  - “No imports / no exec references” (and list ripgrep terms used)
- **Risk level:** low | medium | high
- **If medium/high:** propose a safer alternative (deprecate + stub + TODO) instead of deletion

## Method
1) Use `rg` (ripgrep) across each repo to find references.
2) Check `docker-compose.yml`, `docker-entrypoint.sh`, CI workflows (e.g., `.github/workflows/*`), and any `Makefile`/scripts that orchestrate.
3) Only delete after the plan is printed and validated internally (no user confirmation required, but the plan must exist).

## Output format
### Deletion Candidates
- [ ] **Path:** `...`
  - **Repo:** ...
  - **Why it’s dead:** ...
  - **Proof it’s unused:** ...
  - **Risk:** ...
  - **Safer alternative (if needed):** ...

### Deferred Candidates (do not delete yet)
- [ ] **Path:** `...`
  - **Repo:** ...
  - **Why deferred:** ...
  - **Next step:** ...

## Acceptance
- After deletions, all tests/quickstarts still work.
- No missing file references.
- No broken docs links.
- Preflight + policy are single-source-of-truth in core.

Proceed.

[READY FOR NEXT VIBE]

## History

HISTORY_START
> ✅ FINISHED: 20260221 — Repo clarity pass: added Start Here decision tree, ASCII architecture diagram, Success criteria, Baseline NOT host hardening warning, Core not standalone warning, Stability Contract, Versioning and Compatibility across all 3 repos.
HISTORY_END
