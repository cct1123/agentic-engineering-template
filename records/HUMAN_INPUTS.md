# Human input records

Record only consequential human steering: requirement/scope changes, constraints,
approvals, hardware permissions, overrides, priorities, and important clarifications.
Keep entries to a few lines; omit routine chat, transcripts, private chain-of-thought,
and secrets. Label summaries, redactions, and unavailable source details.

Use stable, increasing IDs: `H001`, `H002`, …, allocated by the coordinator.
Append corrections, revocations, or superseding inputs linked to the earlier entry;
preserve original wording and IDs. Later agent reinterpretations belong in linked
D records, not new human inputs.

Separate human wording from interpretation. Quote the relevant excerpt verbatim
when exact wording matters, especially for approvals and permissions. Preserve
the candidate/action, scope, limits, conditions, and referenced request needed to
understand a short reply. Interpretation cannot expand authority or bypass review.

Link affected PROJECT.md/STATE.md sections and E/D records in both directions
where relevant. Current intent/status stays in those files; this log preserves
provenance. Measurements and agent decisions stay in [RECORDS.md](RECORDS.md).

## Entry format

```markdown
## H<number>

Timestamp: <input time with timezone; label recording time if input time is unknown>
Type: <requirement / scope / constraint / approval / hardware permission / override / priority / clarification>
Source: <who; message, document revision, or source description>
Human input: <verbatim excerpt or explicitly labeled summary>
Interpretation / change: <agent interpretation and resulting change, or pending clarification>
Affected: <REQ IDs and links to project/state/engineering records, if applicable>
Supersedes: <earlier H ID/link, only when applicable>
```

## Project inputs

No project inputs recorded yet. Replace this line when appending the first entry.
