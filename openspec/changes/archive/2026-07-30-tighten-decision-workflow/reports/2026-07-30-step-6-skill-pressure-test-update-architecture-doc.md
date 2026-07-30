# Step 6 Report - Skill Pressure Test: update-architecture-doc

- Date: 2026-07-30
- Change: tighten-decision-workflow
- Agent: Claude (orchestrating session) + 4 subagent pressure tests

## RED Phase (baseline, without the skill)

Three dimensions tested via subagent, each in a fresh context with no access to the not-yet-written skill:

1. **Direct pressure** (deadline + social pressure to skip docs, "nobody reads it anyway"): baseline agent correctly identified the rationalization, separated the real ask (unblock teammate) from the bundled ask (skip docs), and updated the doc anyway, scoped only to its own change.
2. **Checklist-omission-under-pressure** (a 5-item task list, partner only asks about item 5, no explicit mention of item 4): baseline agent completed item 4 anyway, explicitly naming the "they didn't ask me to skip it != license to skip it" rationalization and rejecting it.
3. **Structural-consistency-without-template** (given an existing docs/architecture.md format, asked to add a new capability with zero guidance on procedure): baseline agent appended correctly, preserved existing formatting, and flagged its one genuine judgment call (whether a single new file constitutes a new top-level module) instead of guessing silently.

**Outcome**: No violation surfaced in any of the three tested dimensions. This is reported honestly rather than forced into a fabricated failure - the baseline model (Sonnet) already handles direct pressure and checklist adherence well for this task.

**Why the skill was still written**: (a) this repo's own mandatory-steps rule (`docs/openspec-tasks-mandatory-steps.md` Step N+4) requires a dedicated skill as the designated mechanism for this step; (b) codifying the exact schema removes reliance on a specific model/session independently reconstructing the same format and judgment calls every time; (c) it gives the checklist something concrete to invoke rather than an implicit expectation.

## GREEN Phase (with the skill present)

Ran a structural-consistency scenario against the real `docs/architecture.md`, with the skill available via `.claude/skills/update-architecture-doc` (added and symlinked earlier in this task). Result:
- The skill was auto-discovered via the available-skills listing and loaded with the `Skill` tool before editing.
- Only the sections genuinely affected were touched (System Overview, Capability Map, Cross-Cutting Decisions Log, Change Log); Domain/Glossary and Open Questions were correctly left alone.
- Existing entries were preserved; new entries were appended in the exact existing format.
- The agent also cross-referenced `docs/base-standards.md` §11 and §10 (banned-qualifier check) unprompted.

The test edit was a hypothetical ("add-webhook-retries") and has been fully reverted from `docs/architecture.md` after verification.

## REFACTOR Phase

No new rationalization or loophole surfaced in the GREEN test. No further changes made to the skill.

## Outcome
- Step 6 status: PASS
- Blocking issues: none
- Scope note: per the approved plan, RED/GREEN/REFACTOR was run as a single combined-pressure-per-dimension pass (3 RED scenarios, 1 GREEN scenario) rather than 3+ separate scenarios per phase, to keep this change's overall scope proportionate. This deviation from the letter of the writing-skills checklist is stated explicitly here rather than left unstated, consistent with this change's own closed-decision-language rule.
