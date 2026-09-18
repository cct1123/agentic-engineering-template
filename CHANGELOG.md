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

- Fixed the README diagram connection lines in GitHub dark mode. The diagrams
  set `theme: 'base'`, whose default `lineColor` is `#333333` — that reads at
  12.63:1 on the light canvas but only 1.50:1 on dark `#0d1117` and 1.19:1 on
  dark-dimmed `#22272e`, so the edges were effectively invisible to dark-mode
  readers. Set an explicit `lineColor` of `#768390`, chosen by computing WCAG
  relative-luminance contrast rather than by eye: 3.87:1 on light, 4.88:1 on
  dark, 3.88:1 on dark-dimmed, and 3.32:1 against the base theme's own node fill
  where an edge crosses a node. All clear the 3:1 threshold for non-text
  graphical elements. Only the line colour changes: the theme, the node fills,
  and `fontSize` are all untouched.
- Rewrote the persistent engineering loop prompt. It no longer restates the
  hardware phase chain, stopping conditions, or file list that `AGENTS.md` owns —
  that duplication was a second source of truth and had already gone stale
  (it omitted `records/HUMAN_INPUTS.md` and the Loop continuity reconciliation).
  The prompt now points at `AGENTS.md` and spends its words on what a file cannot
  carry: ownership, working through many actions rather than stopping to report
  after one, deciding routine reversible things without asking, and checkpointing
  before stopping. The one deliberate redundancy is safety-relevant — an explicit
  line not to touch hardware before approval. Dropped the subagent instruction,
  which is the harness's decision and is covered by `AGENTS.md`.
  `.claude/commands/loop.md` is kept byte-identical to the README prompt.
- `ARCHITECTURE.md`: the engineering loop diagram now shows resume reconciliation,
  reading the ruled-out list before choosing an action, recording intent before
  irreversible actions, clearing the in-flight entry on state update, and the
  no-new-evidence path to changing approach or escalating.
- Renamed space-containing paths for portability: `prompt log/` → `prompt-log/`
  and the prompt files within it (`guiding-advise-prompt.txt`,
  `modification-prompt.txt`, `starter-prompt.txt`); updated references in
  `README.md` and `docs/TEMPLATE_REVIEW.md`.
