# Template self-review

Reviewed 2026-09-09 against the supplied creation request, then updated for the
hardware-free development and explicit candidate review workflow. Updated
2026-09-10 for discussion → project setup → persistent engineering loop. Earlier
source prompts are retained as historical material in `prompt log/`; README.md
contains the two canonical prompts.

Scope: tabletop workflow simulations and repository structure checks. The
scenarios below use fictional devices, observations, and IDs to exercise the
instructions. They are not real measurements, executed driver tests, or evidence
that any particular agent will follow the instructions. Production PROJECT.md,
STATE.md, records/RECORDS.md, and outputs/REPORT.md remain uninitialized so the
repository can be reused without stale project claims.

## 1. Minimal initialization

**Input:** In a natural discussion, the human asks for a CSV temperature logger
with at least 10 samples/s, explicit units, and bounded recovery from a dropped
connection. They describe a USB sensor, existing code, a device manual, hardware
access, and known operating limits. No implementation design or plan is supplied.

**Walkthrough:** Apply the README setup prompt: inspect the template and supplied
resources, capture agreed intent and limits in PROJECT.md, and note unresolved
recovery timing as an unknown if the discussion gives no bound. Register REQ-001
through REQ-003 and validation methods in STATE.md as UNTESTED. Keep existing code
and create no speculative directories. If the recovery bound materially affects
acceptance, ask for that bound while preparing independent work. The reusable
loop prompt then selects protocol framing uncertainty as the first gap and drives
a software-only transport test, recording its actual result when executed.

**Review result:** The two prompts connect discussion to a usable checkpoint and
then autonomous engineering. The human can review the captured intent without
writing the project file or planning implementation. Low-risk choices stay with
the agent; significant ambiguities remain visible. With only an objective, derived
criteria belong in STATE.md and are labeled as derived, not agreed human facts.

## 2. Device driver

**Input:** A fictional manual defines request/response framing, units, a timeout,
and a command that reads and clears an error register. A fake transport is available;
the real instrument is not yet accessible.

**Walkthrough:** Extract the relevant interface contract, implement a transport
adapter and driver, and expose a normalized `read_temperature()` interface to the
logger. Plan tests for valid data, malformed frames, timeout, and reconnect. Record
the supplied fictional outcomes as simulation-scoped E records in the scenario;
software criteria can pass within that scope. Real-device behavior remains
UNTESTED (or BLOCKED for an identified unavailable device), and electrical
compatibility remains unvalidated. Overall status stays SOFTWARE_DEVELOPMENT
while useful work remains: logger/application behavior, normal/error paths,
startup/shutdown, integration, configuration/dependencies, and relevant static
checks. Prepare exact physical tests and document assumptions. Only after this
work is exhausted, checkpoint HARDWARE_READY and review the candidate. Package
architecture, implementation, test evidence, limitations, assumptions, expected
behavior, first interactions, physical tests and shutdown/rollback in REPORT.md.
If review finds software gaps, resolve them first. Otherwise checkpoint
AWAITING_HUMAN_REVIEW, request explicit candidate integration approval and stop.
The read-and-clear command is assessed for its state-changing effect before use.

**Review result:** Interface boundaries, implementation, evidence, and current
configuration have clear homes. Passing a fake transport cannot validate the
physical instrument. Available code work continues while device access is blocked.
The review gate is neither a project BLOCKED status nor physical validation.

## 3. Integration failure

**Input:** A driver and logger pass in isolation, but their combined acquisition
stalls. Competing diagnoses are transport loss and concurrent ownership of a
request/response stream.

**Walkthrough:** Reproduce the integrated failure with the smallest two-caller
case. Compare a single caller with two callers on a deterministic fake transport,
holding timeouts and framing constant. In the fictional result, only concurrent
callers mix responses. Record E014 (failure/diagnostic observation), D006 (one owner
for request/response transactions), and the targeted change. Rerun the integration
test and affected timeout/reconnect tests; append E015 for the supplied passing
outcomes. Update current diagnosis and source/configuration references in STATE.md.

**Review result:** The workflow separates observation, diagnosis, decision, fix,
and revalidation. Component PASS results do not conceal integrated FAIL behavior.
The evidence record format retains the failed result after the fix.

## 4. Hardware intervention

**Input:** All useful software work is done and the reviewed candidate has explicit
integration approval. Actual device/configuration identity and compatibility are
checked, then the least consequential useful interactions validate initialization
and state reporting before approved actuation. A discrepancy requires continuity
at a labeled connector to be checked using an approved isolation/test procedure.
The agent cannot perform this measurement remotely; no independent work remains.

**Walkthrough:** Save the supported diagnosis and affected requirement as BLOCKED.
Prepare this precise request, referring to the fictional project's known procedure:

> Under the isolation conditions in approved procedure HW-01, perform its continuity
> check between J2 pin 3 and TP3. Return the resistance in ohms and confirm the
> HW-01 isolation checks passed. This distinguishes an open connection from the
> remaining interface fault; after your result, I will update the diagnosis and
> select the next authorized interface test.

On receiving the result, append human-reported evidence with its limits, clear the
resolved request, recheck applicable system state, and resume the next action. If
the actual project lacked HW-01 or known safe conditions, preparing that procedure
or obtaining those missing facts would precede the measurement request.

**Review result:** The request identifies known facts, the external dependency,
an exact action, expected returned data, and resumption. No invented voltage or
safe operating limit is used. Existing scoped authorization is retained; a pending
request or elapsed time does not authorize further actuation.
Before candidate approval, the agent would instead stop at AWAITING_HUMAN_REVIEW
with the prepared procedure; general device permissions alone would not permit it.

## 5. Agent handoff

**Input:** After interruption, context loss, or agent replacement, discard the
scenario's conversational history and send the same persistent loop prompt.
The successor has PROJECT.md, STATE.md, AGENTS.md, and their referenced resources.

**Checkpoint exercised:** STATE.md identifies the CSV logger objective and driver
→ normalized interface → logger architecture; links driver and logger entry points;
lists protocol tests as PASS at revision A, integrated concurrency as FAIL with
E014, and live-device checks as BLOCKED; identifies the configured timeout and
device-access authority; and names the next action as the two-caller diagnostic
test from scenario 3. Detailed logs remain behind the E014 link.

**Walkthrough:** From the three startup files, identify the objective, architecture,
working and failing behavior, current configuration, evidence pointer, and next
action. Inspect only E014 and relevant artifacts. Reconcile any uncommitted change
and pending operation before repeating tests; a record from revision A cannot
automatically validate an incompatible revision B.

**Review-gate branch:** A successor finding AWAITING_HUMAN_REVIEW and no candidate
approval remains stopped without probing an attached device. If explicit approval
for the candidate and current scope is recorded, resume HARDWARE_VALIDATION without
asking again, then check actual device state. If interrupted at HARDWARE_READY,
finish the software-side review; this state alone grants no device access.

**Review result:** The checkpoint fields support continuation without reconstructing
history or repeating setup. This is a tabletop successor exercise, not a
second-agent execution test.

## 6. Regression

**Input:** A change to improve sustained acquisition modifies timeout handling.
Previously, REQ-003 passed TEST-003 at revision A.

**Walkthrough:** Mark the affected PASS as UNTESTED for revision B pending rerun.
The supplied fictional rerun fails reconnect handling: append E020 and set
REQ-003 to FAIL. Make this regression a current gap, diagnose it, and apply a
focused correction. Append E021 for the supplied successful rerun at revision C
and rerun the affected acquisition/integration tests. Only then restore PASS.
Keep E020 and the earlier passing record as historical evidence.

**Review result:** Configuration changes, stale evidence, observed failure, and
new passing evidence have distinct meanings. Changed calibration assumptions or
criteria follow the same invalidation rule. The diagram's failure path returns
to diagnosis and gap selection rather than proceeding to completion.
During hardware validation, fixes trigger relevant software regressions and
affected physical acceptance tests. Changes outside the approved scope or reviewed
safety assumptions return to candidate review before affected device interactions.

## 7. Completion

**Input:** All required criteria have supplied passing evidence for the final
integrated configuration. A documented cosmetic limitation is outside required
acceptance. Required calibration has current evidence.

**Walkthrough:** Perform final integrated validation using the recorded configuration
and acceptance procedure. On the fictional PASS branch, capture the source
fingerprint, dependency/firmware versions, hardware connections, parameters, and
calibration validity. Fill REPORT.md with the resulting architecture, all criteria
and evidence, prerequisites, operating and shutdown procedures, acceptance commands,
recovery instructions, limits, and decisions. Mark STATE.md VALIDATED only after
the report and configuration are ready.

**Adverse branches:** If final validation fails, append the failure and return to
the gap loop. If the real device is unavailable after candidate approval and no
independent work remains, produce a BLOCKED report with the missing physical test
and exact resumption condition. Before approval, an otherwise hardware-ready
candidate instead awaits review. A simulation PASS or
an unmet required criterion cannot be renamed a non-critical limitation.

**Review result:** The final report distinguishes demonstrated behavior from
implementation, and both failure branches preserve truthful project status.

## 8. Available hardware and software-only projects

**Input:** In one branch, hardware is already connected and general device-read
permission exists at startup. In another, the objective is a software-only API.

**Walkthrough:** The hardware branch still completes hardware-free engineering and
candidate review before any real-device discovery, initialization, read, test or
cleanup; a connected device does not bypass explicit candidate approval. The API
branch uses SOFTWARE_DEVELOPMENT, software acceptance and final validation, then
VALIDATED. It does not create a hardware review request.

**Review result:** The gate controls first physical interaction without imposing
unnecessary human review on projects with no hardware. Hardware-free completion
cannot be reported as fully validated or production-ready for hardware projects.

## Structural verification

The initial template passed 65 structural assertions on 2026-09-09. That result
predates the workflow correction and does not validate the revised files.

Previous result (2026-09-09): **PASS — 138 structural checks**, using local Python
3.9.12 standard-library checks on the revised template. Four local Markdown links
resolve; their targets are all in the seven core files. Both Mermaid graphs have
defined nodes and the expected transitions. Graph reachability confirms that
hardware validation and completion cannot be reached in the phase diagram while
omitting the human review gate. The eight tabletop scenarios above were reviewed
against AGENTS.md and the checkpoint/report guidance. `git diff --check` also
passed for all seven edited documentation files.

This result predates the two-prompt onboarding change; its fingerprints describe
the earlier revision, not validation of the current files.

Core file fingerprints at verification (SHA-256 prefixes of UTF-8 text normalized
to LF): README.md `947386d895b8`; PROJECT.md `754dacacf162`; AGENTS.md `800197bfb673`;
ARCHITECTURE.md `52b9d65e211f`; STATE.md `2f9ec90cadd7`;
records/RECORDS.md `85855f7bf278`; outputs/REPORT.md `afae2a3a8b7e`.

Repository checks cover:

- All seven core files exist and are nonempty.
- Relative Markdown links and heading anchors resolve.
- Relative links resolve with only the seven core files available.
- Fenced blocks and Markdown tables have consistent delimiters and column counts.
- STATE.md and REPORT.md remain NOT_STARTED, with empty requirement tables;
  there is no active human-action request and actual engineering records are empty.
- The Mermaid graphs contain the hardware candidate/review/validation path, the
  core repeat loop, final-validation failure, human-result return, independent
  work, and blocked-handoff paths.

These checks validate the template's structure and initial state. The diagram
uses GitHub Mermaid syntax; graph-path checks are not a browser rendering test.
The scenario outcomes above are instruction-level review, not runtime enforcement.

### Onboarding verification — 2026-09-10

**PASS — 56 structural checks** using a one-off Python standard-library check of
the seven core files and this review document: all 10 local Markdown links and
their anchors resolve, core guidance links require no optional files, fences and
tables are consistent, and the README has exactly two canonical prompt headings.
The objective, project records, and human-action request remain uninitialized.
The hardware phase graph and AGENTS.md from "Hardware project phases" onward
are unchanged from the pre-edit revision. `git diff --check` passes.

Scenarios 1 and 5 were reviewed against the revised onboarding instructions;
scenarios 2, 4, and 8 retain the explicit hardware gate and software-only path.
These are documentation checks and tabletop reviews, not physical tests or agent
execution trials. Review and pruning shortened repeated guidance while keeping
both prompts and the hardware rules intact; the checks were rerun before commit.

## Simplification pass

Kept seven core files. One project definition captures human intent; one current
checkpoint holds requirement status and the short plan. Combined gaps with failures and
priority with next action in STATE.md. Kept evidence and decisions in one record
file, with separate IDs. The final report is a result snapshot, not another live
backlog. There is no scheduler, database, mandatory agent hierarchy, configuration
schema, or speculative directory tree. Project-specific tests and engineering
directories are created when needed. These review notes are optional maintainer
material and are deliberately outside the project's evidence ledger.

The workflow correction keeps that structure. Hardware review uses STATE.md and
REPORT.md rather than a new gate document. Replaced ACTIVE/COMPLETE status guidance
with the explicit phase states and VALIDATED; narrowed the old blanket blocked
handoff rule to exclude planned candidate review. Retained the adaptive engineering
loop, evidence rules, scoped authorization, calibration and physical-action limits.
Only this reusable repository is in scope; no existing project or active engineering
run was inspected or changed. The original starter prompt remains historical source
material, not the current launch instructions.

Onboarding keeps both canonical prompts in README.md and links from related
guidance. Agent setup captures intent and unknowns in PROJECT.md and initializes
STATE.md; the architecture shows these feeding the existing loop. No project
scaffolding was added. Maintenance evidence stays here, leaving project state
and records uninitialized for reuse. The supplied onboarding request is retained
in `prompt log/` alongside the earlier historical requests.

## Human-input provenance review — 2026-09-10

The human-steering request adds one core file, records/HUMAN_INPUTS.md, bringing
the core set to eight files. H entries separate timestamp, type, source, and human
wording from agent interpretation/change and affected requirements/state. Current
intent stays in PROJECT.md, the checkpoint in STATE.md, and E/D evidence and
engineering decisions in RECORDS.md. Earlier verification above predates this change.

Tabletop review (instruction-level, not an agent execution test):

| Case | Expected handling / review result |
| --- | --- |
| Requirement, scope, constraint, or priority change | H entry retains the source; affected project/state sections link back and validation is reassessed before dependent work. |
| Short approval or hardware permission | Retain the referenced candidate/request and exact scope/limits; interpretation cannot add authority or bypass review. |
| Override, correction, revocation, or later clarification | Append and link the earlier entry; update the current checkpoint without rewriting original human wording. Agent reinterpretation stays in D records. |
| Interruption after an H entry but before checkpoint update | Compare H entries with STATE.md's last applied input; reconcile later entries before dependent work. Existing logs without that checkpoint are reconciled once. |
| Routine conversation, reported measurement, or agent design choice | Omit routine chat; measurements and design decisions stay E/D records, linked to H inputs only when relevant. |

Only relevant excerpts are retained; summaries and unavailable source details are
labeled. The new log remains unfilled, as do the project definition, checkpoint,
evidence ledger, and report, so template-maintenance history is not inherited by
new engineering projects.

Critical review added the last-applied H checkpoint to cover interrupted state
updates and kept later agent reinterpretations out of the human-input log. Pruning
removed repeated field instructions from AGENTS.md and shortened HUMAN_INPUTS.md.

Final verification after critical review: **PASS — 75 structural checks** with a
one-off Python standard-library check of the eight core files and this review
document. All 16 local Markdown
links/anchors resolve; fences and tables are consistent; project requirements,
E/D records, and H inputs remain uninitialized; the last applied input is none.
Both canonical prompts, both Mermaid diagrams, and AGENTS.md's hardware phase
rules are unchanged from
`a3eaeea`. `git diff --check` passes. This verifies document structure and the
preserved guidance, not runtime enforcement of human-input recording.
