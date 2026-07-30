## Context

This repo (`lidr-specboot`) is a toolkit of AI-agent development standards, distributed to other projects via `packages/specboot`. OpenSpec was just initialized in this repo for the first time as a prerequisite to this change (see `openspec/config.yaml`). The existing workflow already covers most of a stricter externally-reviewed model via `enrich-us`, the `/ff` planning step (governed by `docs/base-standards.md` §5 and `docs/openspec-tasks-mandatory-steps.md`), `/apply`, `adversarial-review`, and `commit`. Two pieces don't exist yet: a rule banning vague language, and a maintained architecture document. `docs/openspec-tasks-mandatory-steps.md` also currently contains one instance of the exact vague language ("MANDATORY if applicable") this change bans elsewhere.

## Goals / Non-Goals

**Goals:**
- Ban vague, ambiguity-preserving language in OpenSpec artifacts, replacing it with explicit decisions or flagged open questions.
- Introduce a continuously-maintained architecture document and the skill/step that keeps it current.
- Tighten `enrich-us`, `adversarial-review`, and `commit` to match the stricter workflow.
- Keep every skill edit and new skill creation compliant with `ai-specs/skills/writing-skills/SKILL.md`'s RED-GREEN-REFACTOR Iron Law -- one skill at a time, pressure-tested before moving to the next.

**Non-Goals:**
- Bootstrapping OpenSpec itself (prerequisite, already complete before this change's tasks begin).
- Syncing `packages/specboot/template/` (already diverged from this repo independently; a separate, larger effort).
- New Jira/Playwright MCP wiring.
- New implementer/reviewer agent files (`adversarial-review` already covers the reviewer role as a skill).
- Integrating external third-party QA/review tools (e.g. Cursor Backbot, CodeRabbit AI).

## Decisions

- **`docs/architecture.md` skeleton**: Purpose, System Overview, Domain/Glossary, Capability Map (cross-linking `openspec/specs/`), Cross-Cutting Decisions Log, Open Questions, Change Log (one entry appended per archived change). Chosen because it mirrors this repo's existing documentation-standards pattern (a single canonical file per concern) rather than inventing a new mechanism.
- **New skill name**: `update-architecture-doc`, matching the existing `update-docs` naming convention already used in this repo (as opposed to a Superpowers-style gerund like `updating-architecture-doc`).
- **`enrich-us` output format**: Original + Scope + Closed Decisions + Expected Behavior, with the old free-form "Enhanced" section removed entirely rather than kept as a parallel recap -- a recap paragraph restating the same decisions in prose creates a second place those decisions could drift out of sync; removing it leaves exactly one source of truth per decision.
- **Section numbering in `docs/base-standards.md`**: new content is appended as §10/§11 rather than inserted earlier, because `README.md` already cross-references existing §8/§9 by number and inserting mid-file would silently break those references.
- **`openspec/config.yaml` `rules:` keys**: use real artifact IDs (`proposal`, `tasks`) rather than an invented `_global` key -- the schema validates rule keys against real artifact IDs and an unrecognized key produces a CLI warning.

## Risks / Trade-offs

- **Partial Claude Code/Cursor OpenSpec skill install**: this repo's own pre-existing `openspec-sync-specs` skill collides in name with one of OpenSpec's built-in skills, so Claude Code and Cursor only received 3 of 6 built-in OpenSpec skills and no command shortcuts (Codex and Gemini installed cleanly). Explicitly deferred -- accepted as a known gap for this change, not fixed here.
- **First-ever OpenSpec change in this repo**: all six capabilities in this change are "new" from OpenSpec's tracking perspective even though most map to pre-existing skill files, since `openspec/specs/` started empty. This is expected and not a sign of scope creep.
- **Rewording existing text**: changing `docs/openspec-tasks-mandatory-steps.md`'s "MANDATORY if applicable" phrasing changes existing, working guidance; mitigated by making the new applies/does-not-apply rule strictly more explicit, not behaviorally different.
