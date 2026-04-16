# codex-harness-skill

Shareable `workspace-meta-harness` skill for Codex-style agents.

This repository contains a path-sanitized version of the skill used to run a
workspace-first Meta-Harness loop:

- workspace-level learning across projects
- project-level run capture and postmortems
- dual-layer memory (`00~03`)
- Hermes as scheduler/narrator instead of source of truth

## Privacy Audit

This public version intentionally removes or replaces:

- local absolute paths
- usernames and home-directory paths
- machine-specific workspace roots
- live runtime file paths
- concrete run ids and company-specific storage roots

The public skill uses placeholders and environment variables instead:

- `CODEX_SELFOPT_REPO`
- `CODEX_SELFOPT_WRAPPER`
- `CODEX_WORKSPACE_SELFOPT_HOME`
- `TARGET_REPO_ROOT`

## Files

- [SKILL.md](./SKILL.md)
- [references/runtime-map.md](./references/runtime-map.md)

## Suggested Local Variables

```bash
export CODEX_SELFOPT_REPO=/path/to/codex-workspace-selfopt
export CODEX_SELFOPT_WRAPPER=$CODEX_SELFOPT_REPO/selfopt
export CODEX_WORKSPACE_SELFOPT_HOME=$HOME/.codex/workspace-selfopt
```

## Notes

- The skill describes a workflow, not a hosted service.
- Runtime artifacts should remain outside this repo.
- Confirmed memory and candidate memory must stay separate.
