# Engineering state

Agent-maintained current checkpoint, not a chronological log. Replace guidance
with concise facts and links during project setup and as work proceeds. Read
PROJECT.md for human intent; use records/RECORDS.md for evidence and decisions.
Link relevant [H records](records/HUMAN_INPUTS.md) for human steering and authority.

## Status

NOT_STARTED — project objective has not yet been initialized.

Last applied human input: none.

<!-- Hardware path: SOFTWARE_DEVELOPMENT -> HARDWARE_READY ->
AWAITING_HUMAN_REVIEW -> HARDWARE_VALIDATION -> VALIDATED.
SOFTWARE_DEVELOPMENT includes simulation/mocks and software-only validation.
Use HARDWARE_READY only after all meaningful hardware-independent work is done;
review the candidate, then save AWAITING_HUMAN_REVIEW and stop for explicit approval.
This planned gate is not BLOCKED. On resume, do not interact with devices before
candidate approval. Software-only projects go SOFTWARE_DEVELOPMENT -> VALIDATED.
NOT_STARTED and BLOCKED are also valid; for BLOCKED, record the phase to resume.
A blocked requirement or missing hardware does not make the project BLOCKED while
useful independent work remains. Record checkpoint date/time and candidate or
artifact revision/fingerprint after work begins. -->

## Objective

Not initialized. Source: [PROJECT.md](PROJECT.md#engineering-objective).

## Requirements status

No requirements registered yet. Assign IDs from PROJECT.md at initialization.

| ID / source | Short criterion | Validation method / test ID | Status | Current evidence |
| --- | --- | --- | --- | --- |

<!-- Status: PASS = current evidence demonstrates the criterion; FAIL = observed
failure; UNTESTED = no conclusive current validation (including stale evidence);
BLOCKED = validation/progress depends on an identified external prerequisite.
Use links to E records. Preserve full criteria in PROJECT.md; write explicit
derived criteria here only when absent there and label their source "derived".
List every required criterion; do not hide a blocked physical test behind a
passing mock test. Pending physical tests stay UNTESTED (or BLOCKED with an
identified unavailable prerequisite) through the review gate; software PASS is
not physical acceptance. For BLOCKED, link the prerequisite below. -->

## Current system

Not inspected. Add architecture, important interfaces, and artifact entry points.

## Working / validated

No engineering results validated yet.

## Current gaps and known failures

- Objective, available system, and acceptance criteria need initial inspection.
- No observed engineering failures recorded yet; this does not imply tests pass.

## Current configuration

Not inspected. Capture only reproducibility-critical configuration, versions,
hardware/connection details, calibration validity, and authorized operating scope
with its source and conditions. For hardware integration, record explicit candidate
approval, reviewed revision, scope and limits; recheck actual device state only
after the gate. Keep secrets outside this file.

## Current priority and next action

Read PROJECT.md, inspect the available system, register requirements and validation
methods, then choose the most consequential gap. If the objective is still blank,
request it. For hardware projects, exhaust useful hardware-independent work first.
Keep only the current phase, milestone and next few useful actions here.

## Blockers

None assessed yet. Record dependency, affected requirements, independent work
remaining, and the exact condition that clears each blocker. A pending candidate
review belongs below, not here.

## Human action required

<!-- At AWAITING_HUMAN_REVIEW, link the candidate review in outputs/REPORT.md and
its evidence; request explicit hardware integration approval with scope/limits.
Record the expected reply and next action. Do not request physical access while
meaningful hardware-independent work remains. Clear resolved requests and retain
approval in Current configuration with its H record reference. -->

## Completion status

Incomplete: initialization and engineering validation have not begun. Completion
requires current evidence for all required acceptance criteria plus final system
validation and reproducible operating instructions in [the report](outputs/REPORT.md).
Hardware-ready is a candidate milestone; VALIDATED also requires the applicable
physical acceptance tests to pass.
