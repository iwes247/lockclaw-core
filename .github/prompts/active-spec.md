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
**Task:** Repo clarity pass across `LockClaw-baseline`, `lockclaw-appliance`, and `LockClaw-core` (README + cross-links + versioning contract).

**Pseudocode Logic (Architect’s Plan):**
1. **SCAN:** Read the first ~40 lines of each README and record: tagline, “who it’s for,” “what it does NOT do,” and any broken/incorrect cross-links or naming inconsistencies (especially `application` vs `appliance`).
2. **NORMALIZE:** Standardize naming and links everywhere:
   - Repo names exactly as on GitHub: `LockClaw-baseline`, `lockclaw-appliance`, `LockClaw-core`.
   - Replace any “application” references with “appliance.”
   - Ensure consistent casing in badges, links, and headings.
3. **INSERT (ALL REPOS):** Add the same three sections (identical wording) near the top of each README:
   - **Start Here (Pick One):** A simple decision tree mapping user intent to the correct repo.
   - **How the repos fit together:** Include a small ASCII diagram showing Baseline/Appliance consuming Core.
   - **Success looks like:** 4–6 observable outcomes (no commands) so users know what “correct” looks like.
4. **INSERT (CORE ONLY):** Add two clarity sections to `LockClaw-core`:
   - **Stability Contract:** List what is stable “API” (paths/formats/schemas) vs explicitly unstable/internal items.
   - **Versioning & Compatibility:** Define tag strategy (e.g., `core-vX.Y.Z`), what counts as breaking, and how Baseline/Appliance should pin/vend and upgrade.
5. **REINFORCE (BASELINE ONLY):** Add a blunt warning box in the first screenful:
   - “Baseline is NOT host hardening.”
   - “Assumes host is already reasonably secured.”
   - “No published ports by default.”
6. **REINFORCE (CORE ONLY):** Ensure the first screenful states:
   - “Core is not a standalone install target.”
   - “Most users should not clone Core directly.”
7. **METADATA:** Add/verify GitHub “About” descriptions and topics for all repos (especially `LockClaw-core`) to improve scan-and-understand discoverability.
8. **VALIDATE:** Re-open each README and confirm a newcomer can answer in <15 seconds:
   - Which repo should I use?
   - What does this repo do?
   - What does it NOT do?
   - Where do I go next (correct links)?

**Red-Teamer Constraints (Safety Rails):**
- **No Security Claim Drift:** Do not imply baseline secures the host (firewall/ssh/audit belong to appliance scope).
- **No Behavior Changes:** Documentation-only clarity changes; do not alter runtime defaults, privileges, or docker capabilities.
- **No Core-as-Install Confusion:** Core must clearly read as “shared library/policies,” not an install target.
- **IPv6 Wording:** Avoid misleading “only loopback is safe” language—phrase as “bind to localhost/loopback” without implying firewall guarantees.
- **Link Integrity:** All cross-links must resolve and point to the correct repo names; eliminate `LockClaw-application` references entirely.

**Verification Command:**
Open each repo README and confirm:
- The first screen contains “Start Here (Pick One)” + correct cross-links.
- `LockClaw-baseline` contains the “NOT host hardening” warning box prominently.
- `LockClaw-core` clearly states “not standalone install target” prominently and includes Stability Contract + Versioning sections.
- All links resolve (no 404s), and “appliance” naming is consistent everywhere.



## History

HISTORY_START
HISTORY_END
