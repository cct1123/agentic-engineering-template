# Project definition

Human intent, captured by the agent from the project discussion during setup and
reviewed by the human. Direct human edits are also welcome; plain bullets are
enough. Label assumptions and unknowns rather than inventing facts. No human-written
implementation plan is required. See the [setup prompt](README.md#project-setup-prompt).
Link consequential human intent or changes to their [H records](records/HUMAN_INPUTS.md).

## Engineering objective

<!-- Required: What should the completed system accomplish, and for whom? -->

## Requirements / acceptance criteria

<!-- What observable conditions define success? Include units, tolerances, test
conditions, and priorities where relevant. IDs such as REQ-001 are optional;
the agent can assign them in STATE.md without changing your wording here. -->

## Constraints

<!-- Relevant compatibility, operating ranges, timing, environment, budget,
materials, or platform limits. For physical systems: known safety limits,
prohibited actions, and which device operations are already authorized, by
whom, under what conditions. These limits inform hardware-free development;
explicit approval of the hardware-ready candidate precedes integration.
Do not put credentials here. -->

## Available system

<!-- Existing hardware/devices, physical connections, software/firmware, code,
manuals/URLs, datasets, schematics, interfaces, and test equipment. Paths and
references are sufficient; do not copy public manuals into this file. -->

<!-- Hardware need not be accessible at initialization. The agent completes
meaningful hardware-independent work before requesting physical access. -->

## Known unknowns

<!-- Important unresolved facts, assumptions, or decisions. Distinguish what the
agent can investigate from what needs a human answer. Track investigations and
any required human action in STATE.md. -->

## Project-specific context (optional)

<!-- Private facts, sample restrictions, access arrangements, or priorities the
agent cannot independently discover. Reference private material securely and
describe how authorized access is obtained; do not paste secrets. -->
