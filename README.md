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

   > Read `PROJECT.md`, `AGENTS.md`, and `STATE.md`. Take ownership of the engineering objective and continue the engineering loop, updating durable state. For hardware projects, complete meaningful hardware-free work, prepare a hardware-ready candidate, then stop at `AWAITING_HUMAN_REVIEW` for explicit integration approval. Resume hardware work only within recorded candidate approval. Continue until validated or no useful independent work remains.

Known hardware limits and the scope of authorized device operations belong in
PROJECT.md's constraints. Leaving them unspecified does not grant physical
control authority; the agent can still inspect, design, simulate, and write code.
Hardware access is not required to start, and even available devices wait for
explicit approval of the hardware-ready candidate before interaction.

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
    └── REPORT.md          Candidate review, validated result, or blocked handoff
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

This template repository also includes a small `.gitignore`, historical source
requests in `prompt log/`, and maintainer review notes in `docs/TEMPLATE_REVIEW.md`.
The prompt log is reference material; use the launch prompt above and AGENTS.md
for current operating guidance. These extras are not needed to operate a new project.

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

Hardware projects use this default path, with the engineering loop inside each
phase:

**Requirements → Hardware-free development → Simulation / mocks / automated
testing → Hardware-ready candidate → Human review gate → Hardware integration →
Physical validation → Debug / regression as needed → Validated release.**

Complete all meaningful hardware-independent engineering before requesting
physical access: relevant documentation, architecture, drivers and abstractions,
application/GUI, useful mocks, automated normal/error and startup/shutdown tests,
configuration/dependency checks, and preparation of physical validation procedures.
Missing hardware cannot block the project while useful independent work remains.

The corresponding states are `SOFTWARE_DEVELOPMENT` → `HARDWARE_READY` →
`AWAITING_HUMAN_REVIEW` → `HARDWARE_VALIDATION` → `VALIDATED`. At HARDWARE_READY,
perform a final software-side review and prepare the candidate report with test
evidence, limitations, hardware assumptions, expected behavior, exact first device
interactions, physical tests, and safe shutdown/rollback. Resolve software gaps
found in review, then checkpoint AWAITING_HUMAN_REVIEW and stop for explicit human
approval. This is a planned phase boundary, not an error or BLOCKED status.
Software-only projects go directly from SOFTWARE_DEVELOPMENT to VALIDATED after
required validation; they do not need a hardware gate.

## Inspect progress and resume

Read STATE.md for requirement statuses (`PASS`, `FAIL`, `UNTESTED`, `BLOCKED`),
current configuration, priority, next action, and any exact human action needed.
Follow its evidence links for details; records preserve conclusions and methods,
not a transcript or private reasoning.

After an interruption or agent replacement, use the same launch prompt. A fresh
agent reads PROJECT.md, STATE.md, and AGENTS.md, then checks only the referenced
artifacts and evidence it needs. It reconciles unfinished changes and stale
validation before continuing. At AWAITING_HUMAN_REVIEW it remains stopped unless
explicit candidate approval is recorded; a restart or general device permission
does not bypass review. Applicable recorded approval is retained within its scope.
Conversation history is not required. A stopped agent must be restarted by a human
or an external runner; these files preserve progress between sessions.

## Human actions and completion

Analysis and reversible local work normally proceed autonomously. After candidate
approval, identify the actual device/configuration, check development assumptions,
and begin with the least consequential useful interaction, preferably read-only.
Device reads must be authorized and low risk; a read may change state. Validate
initialization and state reporting before controlled actuation. Compare physical
behavior with expectations, diagnose discrepancies, and rerun software regressions
and affected physical tests after fixes. Device writes and physical actuation require
known limits, a suitable system state, and authority for the actual action;
possessing an interface is not permission. Existing authorization remains valid
within its scope. When a controlled action is needed, the agent prepares a
reviewable procedure, requests the precise action or approval, and continues
independent work. Physical dependencies warrant intervention only after meaningful
hardware-independent work is exhausted. It records how to resume after the result
arrives. Changes beyond the candidate's approved scope require renewed review
before affected device interactions.

Completion requires demonstrated acceptance criteria, relevant passing tests,
validated critical interfaces and integration, documented configuration,
calibration where required, and reproducible operation. The agent performs final
validation and fills outputs/REPORT.md with evidence, operating instructions,
and limitations, then marks STATE.md **VALIDATED**. Required physical acceptance
tests must pass before a hardware project is fully validated or production-ready;
before then it may be hardware-ready or software-complete pending hardware
validation. Non-critical limitations may remain if required criteria pass.
Outside the planned review gate, an external dependency can end a session as
**BLOCKED** only when no useful independent work remains, with a useful report
and resumption instructions; it is never reported as validated completion.
