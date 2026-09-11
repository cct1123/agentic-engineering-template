# Autonomous engineering workspace

A small, reusable workspace for an AI agent to take software, hardware, or an
integrated engineering project from an objective to a validated result. The files
provide operating instructions and durable memory. Use an agent that can inspect
files and use your project's tools; the template itself does not run an agent,
schedule work, or grant device access. A single capable agent is sufficient.

## Start a project

**Discussion with agent → project setup → persistent engineering loop.**

1. **Talk to the agent about what you want to build.** Discuss the objective,
   desired behavior, constraints, acceptance criteria, available resources,
   hardware access, safety limits, and important unknowns naturally. Start with
   what you know; the agent helps clarify what matters. You do not need to design
   the implementation or write a detailed engineering plan.
2. **Let the agent configure the engineering workspace.** Use this repository as
   a template, clone it, or copy the core files below into your project.
   Give the agent access to the project directory and relevant code, manuals,
   schematics, data, or references, then send the setup prompt in the discussion.
   Keep credentials outside tracked files.
3. **Run the persistent engineering loop.** Review the captured intent in
   [PROJECT.md](PROJECT.md), correct anything needed, then send the reusable loop
   prompt. The agent chooses the implementation and maintains progress in files.

### PROJECT SETUP PROMPT

```text
Use our preceding discussion and this template to configure this repository for
the engineering task. Read the existing files first. Translate the discussion
into PROJECT.md: objective, desired behavior, observable requirements and
acceptance criteria, constraints, resources, hardware access, permissions, safety
limits, and known unknowns. Preserve human intent and distinguish assumptions
from agreed facts; do not invent permissions or safety limits. Make reasonable
low-risk decisions yourself; ask only about ambiguities that materially affect
scope, acceptance, safety, or a major tradeoff. Adapt the template only where
the project genuinely requires it, preserve existing work and the STATE.md /
records / evidence workflow and hardware review gate, and avoid unnecessary
scaffolding. Initialize STATE.md with requirements, validation methods, and the
next useful action, leaving the repository ready for autonomous engineering.
```

### PERSISTENT ENGINEERING LOOP PROMPT

```text
Read PROJECT.md, AGENTS.md, STATE.md, and relevant project resources. Take
ownership of the engineering objective. Identify and execute the most
consequential useful next action; implement, test, diagnose, refine, and update
durable state and evidence. Use specialists/subagents for bounded tasks when
useful. Continue autonomously until validation is complete, an explicit review
gate is reached, or no useful independent work remains because of a genuine
human decision or external dependency. For hardware: complete hardware-free
development and validation → prepare a hardware-ready candidate → save
AWAITING_HUMAN_REVIEW and stop for explicit candidate approval → perform
authorized physical integration and validation. On resume, honor the gate and
retain recorded candidate approval within its scope.
```

Reuse the loop prompt after interruptions, context loss, or agent replacement.
Project continuity comes from repository state, not chat history; setup is only
needed to configure the project, not each time work resumes.

## Files and ownership

```text
project/
├── README.md             Start, operate, and resume the workspace
├── PROJECT.md            Human intent and available resources
├── AGENTS.md             Agent operating instructions
├── ARCHITECTURE.md       Engineering loop and workspace boundaries
├── STATE.md              Agent's concise current checkpoint
├── records/
│   ├── RECORDS.md         Evidence E001… and decisions D001…
│   └── HUMAN_INPUTS.md    Consequential human inputs H001… and their sources
└── outputs/
    └── REPORT.md          Candidate review, validated result, or blocked handoff
```

Humans supply and review intent; direct edits to PROJECT.md are also welcome.
Agents maintain the checkpoint, records, report, and engineering artifacts.
Record the actual system architecture in STATE.md and the report. Keep existing
project conventions and add directories or design documents only when useful.

Agents preserve consequential steering in [HUMAN_INPUTS.md](records/HUMAN_INPUTS.md):
human wording and source, separate from interpretation, with links to affected
intent, state, or evidence. Routine chat and transcripts are omitted. STATE.md
tracks the last applied input so successors can find newly recorded steering.

The repository also has a `.gitignore`, historical requests in `prompt log/`, and
maintainer notes in `docs/TEMPLATE_REVIEW.md`. These are optional extras; the two
prompts above and AGENTS.md provide current operating guidance.

## How work proceeds

**Requirements → inspect current state → identify the most consequential gap →
choose an action → design / implement → test / measure → diagnose / evaluate →
update state → requirements satisfied? → repeat or validate completion.**

Evidence changes the plan. Each requirement links to a validation method, result,
and evidence for the relevant configuration. The current agent coordinates any
specialists and integrates their results into one checkpoint. See the
[architecture diagram](ARCHITECTURE.md).

Hardware access is not required to start. Complete all meaningful hardware-free
work, then review the candidate and prepare outputs/REPORT.md with evidence,
limitations, assumptions, physical validation procedures, and shutdown/rollback.
Resolve software gaps before saving `AWAITING_HUMAN_REVIEW`. Even connected devices
wait for explicit candidate approval before any interaction; general device
permissions do not bypass the gate. See the [hardware phase rules](AGENTS.md#hardware-project-phases).
Software-only projects use `SOFTWARE_DEVELOPMENT` → `VALIDATED` after required
validation, without a hardware gate.

## Inspect progress and resume

Read STATE.md for requirement statuses (`PASS`, `FAIL`, `UNTESTED`, `BLOCKED`),
current configuration, priority, next action, and any exact human action needed.
Follow its evidence links for details.

A fresh agent using the [persistent loop prompt](#persistent-engineering-loop-prompt)
checks the checkpoint against actual artifacts and relevant evidence, reconciling
unfinished operations and stale validation before continuing. A restart does not
bypass AWAITING_HUMAN_REVIEW; applicable recorded approval is retained within its
scope. A stopped agent must be restarted by a human or an external runner; these
files preserve progress between sessions.

## Human actions and completion

The agent continues independent work until the review gate or a genuine human
decision or external dependency prevents further progress. It records the exact
request and resumption condition in STATE.md. Hardware limits and authorized
operations belong in PROJECT.md; candidate approval covers only its recorded
scope. See [AGENTS.md](AGENTS.md#physical-action-and-human-intervention) for the
operating boundaries and when renewed review is needed.

Completion requires current evidence for all required criteria, final integrated
validation, calibration where required, and reproducible operation. The agent
fills outputs/REPORT.md with evidence, operating instructions, and limitations,
then marks STATE.md **VALIDATED**. Hardware projects also require passing physical
acceptance tests. **AWAITING_HUMAN_REVIEW** is a planned gate; **BLOCKED** is a
handoff when only external dependencies remain. Neither is validated completion.
