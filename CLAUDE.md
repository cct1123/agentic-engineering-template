# Claude Code adapter

This repository's operating instructions live in **[AGENTS.md](AGENTS.md)**. Read
it first; it is the source of truth for how to work here. This file only adapts
that guidance to Claude Code and is intentionally thin — it does not restate the
workflow.

## Where things are

- **[PROJECT.md](PROJECT.md)** — human intent, resources, constraints, and safety limits.
- **[AGENTS.md](AGENTS.md)** — operating instructions, evidence rules, and the hardware review gate.
- **[STATE.md](STATE.md)** — the current checkpoint: requirement status, priority, and next action.
- **[ARCHITECTURE.md](ARCHITECTURE.md)** — the engineering loop and workspace boundaries.
- **records/** — evidence and decisions ([RECORDS.md](records/RECORDS.md)) and consequential human steering ([HUMAN_INPUTS.md](records/HUMAN_INPUTS.md)).
- **[outputs/REPORT.md](outputs/REPORT.md)** — candidate review, validated result, or blocked handoff.

## How to start or resume

- **New project:** run `/setup` (see [.claude/commands/setup.md](.claude/commands/setup.md)) after discussing the objective. It captures intent into PROJECT.md and initializes STATE.md.
- **Continue work:** run `/loop` (see [.claude/commands/loop.md](.claude/commands/loop.md)) to take ownership of the objective and drive the next consequential action.

Both slash commands wrap the canonical prompts in [README.md](README.md); the
README prompts remain authoritative if the two ever differ.

## Honor the review gate

Do not interact with physical devices before explicit candidate approval, even
when general device access exists. Follow the hardware phase rules in
[AGENTS.md](AGENTS.md#hardware-project-phases).

## Maintainer check

`python3 tools/validate_template.py .` validates the template's structure and that
STATE.md / REPORT.md remain uninitialized. It runs in CI on pull requests.
