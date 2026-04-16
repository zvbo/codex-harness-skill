# Runtime Map

## Core Paths

- Skill root: `<this-skill-dir>/`
- Harness code repo: `$CODEX_SELFOPT_REPO`
- Wrapper entrypoint: `$CODEX_SELFOPT_WRAPPER`
- Workspace state root: `$CODEX_WORKSPACE_SELFOPT_HOME`
- Workspace objectives:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/memory/00_objectives.yaml`
- Workspace ideas:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/memory/01_idea_registry.jsonl`
- Workspace roles:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/memory/02_agent_roles.md`
- Workspace synth knowledge:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/memory/03_synth_knowledge.jsonl`
- Workspace status view:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/status/latest.md`
- Confirmed user profile:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/user_profile/profile.yaml`
- User profile candidates:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/user_profile/candidates.jsonl`
- Daily reviews:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/reviews/daily/`
- Weekly reviews:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/reviews/weekly/`
- Session links:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/registry/session_links.jsonl`
- Session review queue:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/registry/session_link_review_queue.jsonl`

## Project Paths

Each integrated repository keeps its own harness state at:

```text
<repo>/.codex-selfopt/
  project_profile.yaml
  memory/
    00_objectives.yaml
    01_idea_registry.jsonl
    02_agent_roles.md
    03_synth_knowledge.jsonl
  status/latest.md
  status/history.md
  status/timeline.jsonl
  runs/<run_id>/
  reviews/<run_id>/
  injection/project_packet.md
```

## Common Commands

Run from any directory through the wrapper:

```bash
$CODEX_SELFOPT_WRAPPER workspace link-sessions
$CODEX_SELFOPT_WRAPPER workspace daily-review --date YYYY-MM-DD
$CODEX_SELFOPT_WRAPPER workspace weekly-review --week-id YYYY-Www
$CODEX_SELFOPT_WRAPPER workspace ignore-run --run-id run-YYYYMMDD-example --repo /path/to/repo
$CODEX_SELFOPT_WRAPPER project run --repo /path/to/repo --objective "..." --task-family general
$CODEX_SELFOPT_WRAPPER project review --repo /path/to/repo
$CODEX_SELFOPT_WRAPPER project history --repo /path/to/repo --since YYYY-MM-DD
$CODEX_SELFOPT_WRAPPER project ignore-run --repo /path/to/repo --run-id run-YYYYMMDD-example
```

Hermes cron checks:

```bash
hermes cron list
hermes cron status
hermes cron run <job_id>
hermes cron tick
```

## Evidence Rules

- `memory/00~03` is the primary state layer.
- `profile.yaml` is confirmed state.
- `reviews/daily/*.json` may contain candidates and provisional signals.
- `reference_only` rows are hints, not durable truth.
- When a summary depends on fewer than 3 real runs, say so explicitly.
