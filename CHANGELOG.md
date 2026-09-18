# Changelog

All notable changes to this template are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- `tools/validate_template.py`: standard-library structural validator for the
  core files, internal Markdown links and anchors, fenced blocks, tables, and the
  uninitialized checkpoint/report state. Makes the review notes' "PASS - N
  structural checks" claim reproducible.
- `.github/workflows/validate.yml`: CI that runs the validator on pushes to
  `main` and on pull requests.
- `LICENSE`: MIT license (update the copyright holder before reuse).
- `CLAUDE.md` and `.claude/commands/setup.md` + `.claude/commands/loop.md`: a thin
  Claude Code adapter and slash commands that wrap the canonical README prompts.
- `CHANGELOG.md`: this file.
- README note pointing to the maintainer tooling above.
- `AGENTS.md`: a "Persistent loop robustness" section governing multi-session
  work — single-writer checkpoint ownership with stale-entry reclaim; recording
  intent before irreversible or long actions; treating an unresolved in-flight
  entry as an UNKNOWN outcome to verify rather than assume; ordering writes
  (records and artifacts, then state, then clear in-flight) so an interruption is
  detectable; stall detection with an escalation ladder; a ruled-out list that
  keeps negative knowledge cheap to find; confirming delegated work before
  repeating it; and bounding effort against a stated limit.
- `STATE.md`: a "Loop continuity" section carrying those rules across sessions —
  session owner / last checkpoint, in-flight action, attempts on current gap,
  effort limit, and a "Ruled out / do not retry" table.
- Validator checks that the loop-continuity fields exist, that the template ships
  with no outstanding in-flight action, and that the robustness rules are present
  in `AGENTS.md`.
- README visual guides: a project lifecycle diagram (discussion → setup → loop →
  review gate → validated) and a persistence diagram showing resume
  reconciliation, intent-before-action, the numbered write order, and the
  no-new-evidence path. Both stay higher level than the detailed diagrams in
  `ARCHITECTURE.md`.

### Changed

- `ARCHITECTURE.md`: the engineering loop diagram now shows resume reconciliation,
  reading the ruled-out list before choosing an action, recording intent before
  irreversible actions, clearing the in-flight entry on state update, and the
  no-new-evidence path to changing approach or escalating.
- Renamed space-containing paths for portability: `prompt log/` → `prompt-log/`
  and the prompt files within it (`guiding-advise-prompt.txt`,
  `modification-prompt.txt`, `starter-prompt.txt`); updated references in
  `README.md` and `docs/TEMPLATE_REVIEW.md`.
