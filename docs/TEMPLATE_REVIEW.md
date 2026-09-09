# Template self-review

Reviewed 2026-09-09 against the supplied `starter prompt.txt`.

Scope: seven tabletop workflow simulations and repository structure checks. The
scenarios below use fictional devices, observations, and IDs to exercise the
instructions. They are not real measurements, executed driver tests, or evidence
that any particular agent will follow the instructions. Production PROJECT.md,
STATE.md, records/RECORDS.md, and outputs/REPORT.md remain uninitialized so the
repository can be reused without stale project claims.

## 1. Minimal initialization

**Input:** One paragraph asks for a CSV temperature logger. Bullets require at
least 10 samples/s, explicit units, and bounded recovery from a dropped connection.
The human lists a USB sensor, existing code, and a device manual. Known operating
limits are supplied; no implementation plan is supplied.

**Walkthrough:** Read the three startup files and inspect the existing code/manual.
Register REQ-001 through REQ-003 in STATE.md with source criteria and proposed
TEST-001 through TEST-003. Mark them UNTESTED. Select protocol framing uncertainty
as the first gap because acquisition and recovery depend on it. Inspect framing
and write a software-only transport test without asking the human to plan work.

**Review result:** The startup and gap-selection instructions support immediate
progress from one edited file. The human need not supply public engineering
background. With only an objective, the agent can mark derived acceptance criteria
in STATE.md; consequential ambiguity is the point for a precise question.

## 2. Device driver

**Input:** A fictional manual defines request/response framing, units, a timeout,
and a command that reads and clears an error register. A fake transport is available;
the real instrument is not yet accessible.

**Walkthrough:** Extract the relevant interface contract, implement a transport
adapter and driver, and expose a normalized `read_temperature()` interface to the
logger. Plan tests for valid data, malformed frames, timeout, and reconnect. Record
the supplied fictional outcomes as simulation-scoped E records in the scenario;
software criteria can pass within that scope. Real-device behavior remains
BLOCKED, and electrical compatibility remains unvalidated. The read-and-clear
command is assessed for its state-changing effect before use.

**Review result:** Interface boundaries, implementation, evidence, and current
configuration have clear homes. Passing a fake transport cannot validate the
physical instrument. Available code work continues while device access is blocked.

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

**Input:** All useful software work is done. Continuity at a labeled connector
must be checked physically using an existing approved isolation and test procedure.
The agent cannot perform this measurement remotely.

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

## 5. Agent handoff

**Input:** Discard the scenario's conversational history. The successor has only
PROJECT.md, STATE.md, AGENTS.md, and the workspace those files reference.

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

**Review result:** The checkpoint fields support continuation without reconstructing
history. This is a tabletop successor exercise, not a second-agent execution test.

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

## 7. Completion

**Input:** All required criteria have supplied passing evidence for the final
integrated configuration. A documented cosmetic limitation is outside required
acceptance. Required calibration has current evidence.

**Walkthrough:** Perform final integrated validation using the recorded configuration
and acceptance procedure. On the fictional PASS branch, capture the source
fingerprint, dependency/firmware versions, hardware connections, parameters, and
calibration validity. Fill REPORT.md with the resulting architecture, all criteria
and evidence, prerequisites, operating and shutdown procedures, acceptance commands,
recovery instructions, limits, and decisions. Mark STATE.md COMPLETE only after
the report and configuration are ready.

**Adverse branches:** If final validation fails, append the failure and return to
the gap loop. If the real device is unavailable, produce a BLOCKED report with
the missing physical test and exact resumption condition. A simulation PASS or
an unmet required criterion cannot be renamed a non-critical limitation.

**Review result:** The final report distinguishes demonstrated behavior from
implementation, and both failure branches preserve truthful project status.

## Structural verification

Result: **PASS — 65 structural assertions**, including four repository links and
four links in an isolated temporary copy of the seven core files. Checks ran with
local Python standard-library tools on 2026-09-09; the temporary copy was removed
after verification. All seven tabletop scenarios have a supported continuation,
completion, or explicit blocked-handoff path in the operating instructions.

Repository checks cover:

- All seven core files exist and are nonempty.
- Relative Markdown links and heading anchors resolve.
- A clean temporary copy of only the seven core files retains valid links.
- Fenced blocks and Markdown tables have consistent delimiters and column counts.
- STATE.md and REPORT.md remain NOT_STARTED, with empty requirement tables;
  the human-action section and actual engineering records are empty.
- The Mermaid graph contains explicit repeat, final-validation failure,
  controlled-action return, independent-work, and blocked-handoff paths.

These checks validate the template's structure and initial state. The diagram
uses GitHub Mermaid syntax; graph-path checks are not a browser rendering test.
The scenario outcomes above are instruction-level review, not runtime enforcement.

## Simplification pass

Kept seven core files. One human input file supplies intent; one current checkpoint
holds requirement status and the short plan. Combined gaps with failures and
priority with next action in STATE.md. Kept evidence and decisions in one record
file, with separate IDs. The final report is a result snapshot, not another live
backlog. There is no scheduler, database, mandatory agent hierarchy, configuration
schema, or speculative directory tree. Project-specific tests and engineering
directories are created when needed. These review notes are optional maintainer
material and are deliberately outside the project's evidence ledger.
