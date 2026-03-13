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

[ARCHITECT]

Below is a ready-to-paste active-spec.md prompt for your workstation AI so you can preserve momentum now and execute in ~8 days.

Workstation Handover — active-spec.md

Task: Reposition the LockClaw repo so it clearly separates workspace structure/routing from runtime enforcement, while cleaning dead/duplicative repo content and improving the README/docs flow.

Intent: Borrow the useful “folder as workspace” idea without diluting LockClaw into a generic workflow system. LockClaw must remain clearly framed as policy-before-autonomy, fail-closed, deny-by-default, and runtime enforcement, not merely organization.

⸻

Objective

Refactor the repo so a new visitor immediately understands:
	1.	What LockClaw is
	•	enforcement layer for AI/agent systems
	•	fail-closed runtime boundaries
	•	least privilege
	•	policy controls over tools/filesystem/network
	2.	What LockClaw is not
	•	not just a prompt library
	•	not just folder conventions
	•	not just markdown workflow
	•	not a generic mega-agent framework
	3.	How workspace structure fits
	•	structured folders/markdown/routing are useful
	•	but they are the cognitive/workflow layer
	•	LockClaw sits underneath as the control/enforcement layer

⸻

Architecture Direction

Use this framing throughout the repo:
	•	Workspace Layer
	•	map
	•	task routing
	•	human-readable instructions
	•	markdown specs
	•	file organization
	•	Enforcement Layer
	•	tool restrictions
	•	filesystem boundaries
	•	network controls
	•	allowlists
	•	fail-closed policy checks
	•	least privilege execution
	•	Verification Layer
	•	validation
	•	auditability
	•	dependency checks
	•	safe failure behavior

LockClaw should be described as:
“Routing decides what the AI sees. LockClaw decides what the AI can do.”

⸻

Required Repo Changes

1. Root README rewrite
Rewrite the root README so it:
	•	leads with the enforcement thesis
	•	clearly distinguishes LockClaw from workflow-only systems
	•	explains why structure alone is not enough
	•	introduces the 3-layer mental model:
	•	workspace
	•	enforcement
	•	verification
	•	includes a clean “Why LockClaw” section
	•	includes a “What problem this solves” section
	•	includes a “What LockClaw does not try to be” section

2. Repo cleanup
Audit the repository and:
	•	identify dead code
	•	identify stale docs
	•	identify duplicated README sections
	•	identify old concepts from before the repo direction changed
	•	remove or consolidate anything that confuses the current mission

Do not preserve legacy material just because it exists. Prefer clarity over nostalgia.

3. Docs structure cleanup
Create or normalize docs so the repo has a clear information path:
	•	overview / mission
	•	architecture
	•	threat model
	•	enforcement model
	•	workspace integration guidance
	•	contributor/developer flow

Use concise markdown. Human-readable first.

4. Positioning update
Throughout the repo, replace vague “AI safety helper” language with sharper positioning:
	•	deny-by-default
	•	enforcement runtime
	•	fail-closed
	•	least privilege
	•	policy-before-autonomy
	•	secure-by-default agent execution

5. Workspace compatibility section
Add a docs section that explicitly says LockClaw works with:
	•	structured folder workspaces
	•	markdown task specs
	•	routing systems
	•	selective tool loading

But also state clearly:
	•	these improve organization and context selection
	•	they do not provide enforcement
	•	they do not create hard runtime boundaries
	•	they do not replace policy checks

6. Landing-page / repo visual coherence
Make the repo feel cohesive and modern, but aligned to the OpenClaw ecosystem look/feel where appropriate. Prioritize:
	•	clean hierarchy
	•	strong headline
	•	fast comprehension
	•	less clutter
	•	less “prototype energy”
	•	more “real system with clear philosophy”

⸻

Pseudocode Logic

PHASE 1 — SCAN
	•	READ root README
	•	READ docs tree
	•	READ package/config/repo structure
	•	IDENTIFY duplicate messaging
	•	IDENTIFY dead files and stale concepts
	•	IDENTIFY places where LockClaw is framed too vaguely

PHASE 2 — CLASSIFY
	•	TAG each file as:
	•	KEEP
	•	MERGE
	•	REWRITE
	•	DELETE
	•	GROUP content by:
	•	mission
	•	architecture
	•	enforcement
	•	setup
	•	workspace integration
	•	contributor flow

PHASE 3 — REFRAME
	•	REWRITE messaging around enforcement-first positioning
	•	INSERT distinction between:
	•	organization/workflow
	•	enforcement/runtime control
	•	REMOVE language implying LockClaw is only structure, prompts, or chat guidance

PHASE 4 — RESTRUCTURE
	•	NORMALIZE docs layout
	•	MOVE content into obvious locations
	•	REDUCE README bloat
	•	LINK outward to focused docs pages instead of stuffing everything into root README

PHASE 5 — VALIDATE
	•	CHECK that a new user can answer within 60 seconds:
	•	what LockClaw is
	•	what layer it operates in
	•	why routing/markdown are not enough
	•	why fail-closed matters

PHASE 6 — FINALIZE
	•	PRODUCE cleaned tree summary
	•	PRODUCE list of removed files/content
	•	PRODUCE concise PR summary
	•	PRODUCE recommended next-step issues

⸻

Content Requirements

The rewritten content must communicate these durable points:
	•	AI systems fail when they rely only on giant prompts and chat memory
	•	structured workspaces help context selection
	•	routing is useful
	•	markdown is useful
	•	but structure is not enforcement
	•	readable instructions are not hard boundaries
	•	LockClaw exists to enforce boundaries at runtime
	•	the real gap in many agent systems is not “more context,” but “controlled execution”

⸻

File/Doc Targets

Prioritize these paths conceptually:
	•	/README.md
	•	/docs/architecture.md
	•	/docs/threat-model.md
	•	/docs/enforcement-model.md
	•	/docs/workspace-integration.md
	•	/docs/contributing.md

If equivalent files already exist, reuse and consolidate rather than duplicating.

⸻

Acceptance Criteria

The change is complete when:
	1.	The README clearly states LockClaw’s mission in the first screenful.
	2.	The repo no longer reads like a vague agent helper or generic automation project.
	3.	There is a clear explanation of how LockClaw complements structured workspaces instead of competing with them.
	4.	Dead code and stale docs are removed or explicitly deprecated.
	5.	The docs flow is simple enough for a new visitor to navigate from mission → architecture → enforcement → usage.
	6.	The repo language consistently reflects deny-by-default and fail-closed thinking.

⸻

Red-Team Constraint Set

The workstation AI must not:
	•	blur workflow organization with enforcement
	•	preserve stale files just to avoid deleting things
	•	use vague marketing language in place of precise security language
	•	describe markdown/folders/routing as if they provide actual runtime protection
	•	weaken fail-closed positioning to sound more mainstream
	•	ignore contributor confusion caused by old repo structure

The workstation AI must:
	•	call out any missing threat-model docs
	•	flag any feature claims that lack enforcement backing
	•	preserve least-privilege and deny-by-default framing
	•	note where docs imply capabilities the current codebase does not actually enforce
	•	treat “clarity gap” as a real security/product risk

⸻

Dependency / Audit Requirements

Before making repo sync changes, verify:
	•	git config user.name is set to iwes247
	•	repo branch state is clean and intentional
	•	no important security docs are removed without replacement
	•	deleted files are logged in the PR notes
	•	renamed/moved docs do not break internal navigation

Also audit for conceptual bypasses:
	•	places where the docs imply safety without enforcement
	•	places where “secure” means only “organized”
	•	places where tools or workflows are described without policy boundaries
	•	any omission of filesystem/network/tool restrictions where they are central to the value proposition

⸻

Verification Plan

The workstation AI should finish by producing:
	1.	A before/after repo structure summary
	2.	A list of deleted/merged/reworked files
	3.	A README diff summary in plain English
	4.	A one-paragraph explanation of how the updated repo distinguishes:
	•	workspace organization
	•	enforcement/runtime controls
	5.	A PR title
	6.	A 3–5 bullet PR description
	7.	A fail-fast note listing anything still unclear, overstated, or not yet enforced in code

⸻

Suggested PR Title

Refocus LockClaw around enforcement-first architecture and clean repo structure

Suggested PR Summary
	•	Clarifies LockClaw as a deny-by-default enforcement layer, not just a workflow system
	•	Cleans stale repo content and reduces duplicate or confusing documentation
	•	Adds clearer architecture/docs separation between workspace routing and runtime enforcement
	•	Improves first-time visitor understanding of mission, threat model, and value proposition
	•	Prepares the repo for stronger contributor alignment and future implementation work

⸻

[RED-TEAMER]

Sharp audit

This prompt is strong because it protects your positioning while still borrowing the useful workflow idea. The main risk is the workstation AI doing a “docs beautification pass” instead of a real mission-hardening pass.

Fail-fast requirements
	•	If the README still sounds like “organized agent workflows,” it failed.
	•	If the docs do not clearly say structure is not enforcement, it failed.
	•	If dead code/stale docs remain because of caution or laziness, it failed.
	•	If the repo claims security outcomes that the implementation does not yet enforce, that mismatch must be explicitly called out.

Keep this principle intact:
Borrow the workspace idea as input hygiene; keep LockClaw centered on enforcement power, not organizational aesthetics.

## History

HISTORY_START
> ✅ FINISHED: 20260226 — Rewrote `LockClaw/README.md` as a landing-page style router with hero/tagline/nav, Start Here chooser, quick baseline-first routing, scope boundaries, repo-fit diagram, contributing pointer, and legacy pointer. Added `docs/assets/lockclaw-mark.png` placeholder path and `docs/legacy/README.md`.
> ✅ FINISHED: 20260226 — Centralized contributing workflow in `lockclaw-core/CONTRIBUTING.md`, removed duplicated “vibe-sync” blocks from core/appliance READMEs, and added short pointer + fail-closed reminder in baseline/appliance READMEs.
> ✅ FINISHED: 20260223 — LockClaw v1 enforcement + cleanup pass completed across core, baseline, appliance. Added core fail-closed pre-flight and hobby/builder mode policies, removed duplicate baseline preflight, enforced read-only root + /data-only writable posture in baseline compose/entrypoint, aligned baseline/appliance docs, and synced vendored core policy artifacts.
> ✅ FINISHED: 20260221 — Repo clarity pass: added Start Here decision tree, ASCII architecture diagram, Success criteria, Baseline NOT host hardening warning, Core not standalone warning, Stability Contract, Versioning and Compatibility across all 3 repos.
HISTORY_END
