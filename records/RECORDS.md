# Engineering records

No project records yet. Append evidence and consequential decisions below as work
occurs. These are durable conclusions and their basis, not a transcript or private
reasoning. STATE.md links directly to the records needed for the current checkpoint.

Use monotonically increasing IDs: `E001`, `E002`, … for evidence; `D001`, `D002`, …
for decisions. Never reuse IDs or overwrite an earlier result to make it pass.
Append a new record for a rerun, correction, or superseding decision and link back.
Keep IDs stable when splitting this file later; update inbound links. Allocate IDs
through the coordinator when agents work concurrently.

Use stable `TEST-001` identifiers for validation procedures and `REQ-001` for
requirements; one test may address multiple requirements and may have many E
records across runs. Link scripts, source, raw data, logs, calculations, manuals,
and configurations rather than embedding large artifacts. Cite relevant manual
sections and versions. Redact secrets before writing any persistent artifact.

## Evidence record format

Copy and fill this format when recording an actual observation. Placeholders and
examples are not evidence. Omit fields only when they are inapplicable, explaining
any material limitation.

```markdown
## E<number>

Date: <timestamp with timezone>
Kind / scope: <test, measurement, inspection, simulation, calibration, human report>
Requirements / test: <REQ IDs; TEST ID and procedure/script link where applicable>
Claim: <narrow statement this observation supports or refutes>
Method: <reproducible command or procedure; inputs, expected result, conditions>
Configuration: <source revision or file hashes; tool/firmware versions; hardware
identity, connections, parameters, and calibration that affect this observation>
Result: <actual values with units, errors, sample counts, and PASS/FAIL/INCONCLUSIVE
against the expected criterion; do not equate command exit status with acceptance>
Artifacts / references: <links to code, logs, raw data, calculations, or docs>
Limitations: <simulation vs physical scope; uncertainty; untested conditions>
Bearing: <effect on requirement status, diagnosis, or next action; previous E IDs>
```

Calibration evidence also states the calibrated quantity, method, reference,
result, uncertainty where meaningful, and validity assumptions. Human-reported
results identify the supplied procedure and result, and their verification limits;
do not describe them as direct agent measurements.

## Decision record format

Record decisions only when their consequences or rationale matter for later work.
Routine edits do not need decision entries.

```markdown
## D<number>

Date: <timestamp with timezone>
Decision: <chosen design, configuration, or diagnostic conclusion>
Basis: <E IDs, source references, or explicit assumptions; affected REQ IDs>
Consequence: <engineering tradeoff, affected interfaces/artifacts, validation needed>
Reconsider if: <new evidence or changed conditions that would invalidate the choice>
Supersedes: <earlier D ID if applicable>
```

## Project records
