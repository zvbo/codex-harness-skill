---
name: workspace-meta-harness
description: >
  Workspace-first Codex self-optimization and Meta-Harness workflow for long-lived
  multi-project environments. Use when the task involves cross-project learning,
  workspace or project reviews, run postmortems, lesson promotion, user preference
  extraction, capability analysis, noisy-run handling, or Hermes scheduled summaries.
---

# Workspace Meta-Harness

Operate a Codex self-optimization system that separates workspace-level learning
from project-level harnessing.

## Use This Skill For

- workspace-level learning across multiple projects
- project-level harness runs, postmortems, and lesson candidates
- extracting or promoting confirmed user preferences
- diagnosing repeated failures, validation gaps, or noisy runs
- updating the harness implementation itself
- Hermes scheduled summaries driven by review artifacts

## Required Setup

Configure these variables or their local equivalents:

- `CODEX_SELFOPT_REPO`
- `CODEX_SELFOPT_WRAPPER`
- `CODEX_WORKSPACE_SELFOPT_HOME`

Project-local state is expected under:

- `<repo>/.codex-selfopt/`

## Source of Truth

- Harness code repo: `$CODEX_SELFOPT_REPO`
- Wrapper: `$CODEX_SELFOPT_WRAPPER`
- Workspace state: `$CODEX_WORKSPACE_SELFOPT_HOME`
- Workspace memory:
  - `memory/00_objectives.yaml`
  - `memory/01_idea_registry.jsonl`
  - `memory/02_agent_roles.md`
  - `memory/03_synth_knowledge.jsonl`
- Confirmed user profile:
  - `user_profile/profile.yaml`
- Project state:
  - `<repo>/.codex-selfopt/`
- Project memory:
  - `<repo>/.codex-selfopt/memory/00_objectives.yaml`
  - `<repo>/.codex-selfopt/memory/01_idea_registry.jsonl`
  - `<repo>/.codex-selfopt/memory/02_agent_roles.md`
  - `<repo>/.codex-selfopt/memory/03_synth_knowledge.jsonl`
- Human-readable status:
  - `$CODEX_WORKSPACE_SELFOPT_HOME/status/latest.md`
  - `<repo>/.codex-selfopt/status/latest.md`
  - `<repo>/.codex-selfopt/status/history.md`

Read [references/runtime-map.md](references/runtime-map.md) for the generic path
and command map.

## Operating Rules

1. Read-only first.
Inspect relevant review JSON, profile, manifests, and run artifacts before making
changes.

2. Separate the layers.
- Workspace layer: user profile, capability matrix, shared lessons, daily and weekly synthesis.
- Project layer: events, manifests, postmortems, lesson candidates, project packets.
- Primary state lives in `memory/00~03`; legacy review and promote files are compatibility views.

3. Preserve governance boundaries.
- `profile.yaml` is authoritative.
- Daily review candidates are provisional until promoted.
- Do not auto-write durable rules into AGENTS, skills, templates, validators, or Hermes memory without explicit approval.

4. Be conservative with weak evidence.
- If `total_runs < 3`, capability conclusions are provisional.
- If a run is noisy or known-bad, call that out before using it as evidence.

5. Keep Hermes in the right role.
Hermes schedules and narrates. The harness stores artifacts and review state.

## Standard Workflow

1. Classify the task:
- workspace synthesis
- project review
- harness implementation
- Hermes summary integration

2. Load only the needed artifacts.
Do not bulk-load all reviews or all project history.

3. Read dynamic memory in this order:
- workspace `memory/02_agent_roles.md`
- workspace `memory/00_objectives.yaml`
- project `memory/02_agent_roles.md`
- project `memory/00_objectives.yaml`
- relevant `01_idea_registry.jsonl` rows
- relevant `03_synth_knowledge.jsonl` rows
- then, only if needed, `status/latest.md`, `status/history.md`, packets, postmortems, and raw artifacts

4. Use the wrapper for operational commands from any directory.

5. When changing harness code, verify with the smallest relevant set of:
- `pnpm typecheck`
- `pnpm test`
- `pnpm build`

6. In summaries, clearly distinguish:
- confirmed profile facts
- candidate patterns
- reference-only hints
- provisional capability judgments

## Command Entry Points

- Workspace review:
  - `selfopt workspace link-sessions`
  - `selfopt workspace daily-review`
  - `selfopt workspace weekly-review`
- Noisy-run handling:
  - `selfopt workspace ignore-run`
  - `selfopt project ignore-run`
- Project loop:
  - `selfopt project init`
  - `selfopt project run`
  - `selfopt project review`
  - `selfopt project inject`
  - `selfopt project history`
- Hermes operations:
  - `hermes cron list`
  - `hermes cron status`
  - `hermes cron run <job_id>`

## Sharing Boundary

This public skill must stay free of live runtime data. Do not commit:

- absolute workstation paths
- home directory names
- live `profile.yaml` contents
- real review outputs
- company-specific storage roots
- machine-specific run ids unless they are purely illustrative placeholders
