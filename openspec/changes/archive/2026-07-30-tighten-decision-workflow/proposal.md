## Why

This repo's existing OpenSpec-based workflow (`enrich-us` -> `/ff` planning -> `/apply` -> `adversarial-review` -> `commit`) already covers most of a stricter AI-assisted development model reviewed externally, but looser in places: there is no rule against vague, ambiguity-preserving language in specs/tasks ("if applicable", "if needed", "preferred", "maybe"), no continuously-maintained architecture document that gives new agent sessions shared context without re-reading the whole codebase, `adversarial-review` stops at a verdict instead of looping back into the implementer, and `commit`'s PR descriptions are less detailed than desired. This change closes those specific gaps.

This is the first OpenSpec change in this repo (OpenSpec was just initialized here as a prerequisite), so every capability below is a new, formal spec for behavior that mostly already existed informally as skill files.

## What Changes

- Add a repo-wide rule banning vague/ambiguous qualifiers in specs, proposals, and tasks; any such phrase must become an explicit decision or an explicit flagged "Open Question" (`docs/base-standards.md` new §10).
- Add a continuously-maintained architecture document (`docs/architecture.md`) and a new skill (`update-architecture-doc`) that keeps it current, with a new mandatory task-authoring step requiring it (`docs/base-standards.md` new §11, `docs/openspec-tasks-mandatory-steps.md` updated).
- Fix the one existing instance of banned vague language already in this repo's own governance doc ("MANDATORY if applicable" in `docs/openspec-tasks-mandatory-steps.md`), replacing it with an explicit applies/does-not-apply rule.
- Tighten `enrich-us`: read the architecture doc first, cross-check against existing code, ask clarifying questions until no ambiguity remains, and change its output format to Original + Scope + Closed Decisions + Expected Behavior (dropping the old free-form "Enhanced" section).
- Tighten `adversarial-review`: add an explicit loop-back-to-implementer step so a change cannot be archived while a Blocker or Major finding is open.
- Tighten `commit`: expand the PR description to cover per-layer changes, tests added by type, evaluations applied, and a link to the OpenSpec change folder.
- Sync `README.md` to reflect all of the above.

## Capabilities

### New Capabilities
- `closed-decision-language`: the rule banning vague/ambiguous qualifiers in OpenSpec artifacts, with an explicit list of banned phrases and the requirement to resolve each into a decision or a flagged open question.
- `architecture-doc-maintenance`: the living `docs/architecture.md` document plus the `update-architecture-doc` skill and the mandatory task-authoring step that keeps it current.
- `user-story-enrichment`: the `enrich-us` skill's behavior -- reading architecture context, checking consistency with existing code, closing ambiguity via guided questions, and producing a Scope/Closed Decisions/Expected Behavior write-up.
- `adversarial-review`: the `adversarial-review` skill's behavior, including the new loop-back-to-implementer requirement until no Blocker/Major finding remains.
- `commit-pr-summary`: the `commit` skill's behavior for producing a detailed, per-layer, evaluations-inclusive pull request description.
- `openspec-task-planning`: the mandatory structure, verification checklist, and applicability rules that `docs/openspec-tasks-mandatory-steps.md` enforces when authoring `tasks.md` for any change.

### Modified Capabilities
(none -- `openspec/specs/` is currently empty; this is the first change, so every capability above is new from OpenSpec's own tracking perspective even though most map to pre-existing skill files.)

## Impact

- Files: `docs/base-standards.md`, `docs/openspec-tasks-mandatory-steps.md`, `docs/architecture.md` (new), `ai-specs/skills/update-architecture-doc/SKILL.md` (new), `ai-specs/skills/enrich-us/SKILL.md`, `ai-specs/skills/adversarial-review/SKILL.md`, `ai-specs/skills/commit/SKILL.md`, `README.md`.
- Symlinks: `.claude/skills/update-architecture-doc` and `.cursor/skills/update-architecture-doc` (new, via `sync-agent-symlinks`).
- Explicitly out of scope: OpenSpec bootstrap itself (prerequisite, already done); `packages/specboot/template/` (already diverged from this repo independently); any new Jira/Playwright MCP wiring; any new implementer/reviewer agent files; integration of external third-party QA/review tools (e.g. Cursor Backbot, CodeRabbit AI).
