# Autonomous engineering workspace

A small, reusable workspace for an engineering agent to take a project from an
objective to a validated result. It supports hardware, electronics, instrumentation,
embedded firmware, device protocols, APIs, automation, acquisition, calibration,
and integrated hardware/software systems.

The files provide operating instructions and durable memory. Run them with a
capable engineering agent that can inspect files and use your project's tools;
the template itself does not run an agent, schedule work, or grant device access.
A single agent can run the entire workflow.

## Start a project

1. Use this repository as a template, clone it, or copy the seven core files below
   into your project. Keep existing code and conventions.
2. Edit [PROJECT.md](PROJECT.md). At minimum, supply an engineering objective.
   Add observable acceptance criteria, constraints, available resources, and any
   private facts the agent cannot discover. A paragraph and a few bullets usually
   suffice; no implementation plan is required.
3. Add or point to existing code, manuals, schematics, data, and hardware details.
   Create `inputs/` only if useful. Keep credentials outside tracked files.
4. Start your agent in the project directory with this prompt:

   > Read `PROJECT.md`, `AGENTS.md`, and `STATE.md`. Take ownership of the engineering objective. Continue the inspect–gap–design–implement–test–diagnose loop, updating durable state, until requirements are validated or a genuine external blocker requires human intervention.

Known hardware limits and the scope of authorized device operations belong in
PROJECT.md's constraints. Leaving them unspecified does not grant physical
control authority; the agent can still inspect, design, simulate, and write code.

## Files and ownership

```text
project/
├── README.md             Start, operate, and resume the workspace
├── PROJECT.md            Human intent and available resources
├── AGENTS.md             Agent operating instructions
├── ARCHITECTURE.md       Engineering loop and workspace boundaries
├── STATE.md              Agent's concise current checkpoint
├── records/
│   └── RECORDS.md         Evidence E001… and decisions D001…
└── outputs/
    └── REPORT.md          Validated result or explicit blocked handoff
```

Humans normally edit only **PROJECT.md**, and provide relevant existing files.
Agents maintain **STATE.md**, **records/RECORDS.md**, **outputs/REPORT.md**, and
the engineering artifacts they create. README.md, AGENTS.md, and ARCHITECTURE.md
are reusable operating guidance; adapt them only when the project needs it.
Put the actual engineered system architecture in STATE.md and the report, with
a separate design document only when its complexity warrants one.

Expand only as needed: `inputs/`, `hardware/`, `interfaces/`, `software/`,
`firmware/`, `tests/`, `measurements/`, `analysis/`, or `docs/`. Existing project
layouts take precedence over these example names.

This template repository also includes a small `.gitignore`, the original
`starter prompt.txt`, and maintainer review notes in `docs/TEMPLATE_REVIEW.md`.
They are not needed to operate a new project.

## How work proceeds

**Requirements → inspect current state → identify the most consequential gap →
choose an action → design / implement → test / measure → diagnose / evaluate →
update state → requirements satisfied? → repeat or validate completion.**

The coordinator is a role the current agent performs. Optional specialists can
work on bounded tasks; the coordinator integrates their results into one state.
See the [architecture diagram](ARCHITECTURE.md).

Choose the action most likely to close the most consequential gap at reasonable
cost and risk. Evidence changes the plan. Do not rebuild working components or
keep polishing requirements that already pass without a concrete reason.
**Implemented does not mean validated:** each important requirement links to a
test, its result, and evidence for the relevant configuration.

## Inspect progress and resume

Read STATE.md for requirement statuses (`PASS`, `FAIL`, `UNTESTED`, `BLOCKED`),
current configuration, priority, next action, and any exact human action needed.
Follow its evidence links for details; records preserve conclusions and methods,
not a transcript or private reasoning.

After an interruption or agent replacement, use the same launch prompt. A fresh
agent reads PROJECT.md, STATE.md, and AGENTS.md, then checks only the referenced
artifacts and evidence it needs. It reconciles unfinished changes and stale
validation before continuing. Conversation history is not required. A stopped
agent must be restarted by a human or an external runner; these files preserve
progress between sessions.

## Human actions and completion

Analysis and reversible local work normally proceed autonomously. Device reads
must be authorized and low risk. Device writes and physical actuation require
known limits, a suitable system state, and authority for the actual action;
possessing an interface is not permission. Existing authorization remains valid
within its scope. When a controlled action is needed, the agent prepares a
reviewable procedure, requests the precise action or approval, and continues
independent work. It records how to resume after the result arrives.

Completion requires demonstrated acceptance criteria, relevant passing tests,
validated critical interfaces and integration, documented configuration,
calibration where required, and reproducible operation. The agent performs final
validation and fills outputs/REPORT.md with evidence, operating instructions,
and limitations. Non-critical limitations may remain if required criteria pass.
An external dependency can end a session as **BLOCKED**, with a useful report
and resumption instructions; it is never reported as validated completion.
