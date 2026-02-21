# lockclaw-core

Shared policy definitions, audit scripts, and port allowlists consumed by [lockclaw-baseline](https://github.com/iwes247/lockclaw-baseline) and [lockclaw-appliance](https://github.com/iwes247/lockclaw-appliance).

This is **not** a standalone project. It is intended to be vendored (`lockclaw-core/`) or added as a git submodule in each consuming repo.

## Contents

| Path | Purpose |
|------|---------|
| `audit/audit.sh` | Validate that required policy files exist and contain correct values |
| `audit/port-check.sh` | Verify no unexpected ports are listening (hard-fail, allowlist-driven) |
| `policies/ports/` | Per-profile port allowlists (JSON) |
| `policies/ssh-requirements.txt` | Required SSH posture values |
| `policies/sysctl-requirements.txt` | Required sysctl values (appliance only) |
| `scanner/security-scan.sh` | Wrapper for AIDE + rkhunter + Lynis (appliance runtime) |
| `docs/threat-model-template.md` | Template for per-repo threat models |

## Usage

### From lockclaw-baseline

```bash
# Vendored at ./lockclaw-core/
./lockclaw-core/audit/port-check.sh --profile container
```

### From lockclaw-appliance

```bash
# Vendored at ./lockclaw-core/
./lockclaw-core/audit/audit.sh --overlay-dir ./overlays
./lockclaw-core/audit/port-check.sh --profile appliance
./lockclaw-core/scanner/security-scan.sh
```

## Versioning

Tag releases as `core-vX.Y.Z`. Consuming repos should pin to a specific version and update deliberately.

## Contributing — vibe-sync workflow

This project uses a phone-to-VSCode bridge for development:

1. **From your phone** — Edit `.github/prompts/active-spec.md` via GPT, commit and push.
2. **At your workstation** — Run `lets-go` (PowerShell) or `./scripts/vibe-sync.sh` to pull the spec.
3. **Copilot executes** — VS Code Copilot reads the active spec and implements the task.
4. **Sync back** — Run `sync-vibe` to archive the completed task and push state back for your phone.

## License

MIT — see [LICENSE](LICENSE).
