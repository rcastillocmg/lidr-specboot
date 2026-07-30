## 0. Setup: Create Feature Branch (MANDATORY - FIRST STEP)

- [x] 0.1 Rename branch to `tighten-decision-workflow` (from `decision-refinement-skill`, created earlier off `main`)
- [x] 0.2 Verify branch creation and current branch status

## 1. Bootstrap OpenSpec (prerequisite, bundled as first task group per user decision)

- [x] 1.1 Upgrade global OpenSpec CLI to latest (1.6.0 -> 1.7.0)
- [x] 1.2 Run `openspec init --tools claude,cursor,codex,gemini` at repo root
- [x] 1.3 Hand-author `openspec/config.yaml` (schema, self-referential context, rules keyed by real artifact IDs)
- [x] 1.4 Run `openspec validate` / `openspec doctor`; confirm clean
- [x] 1.5 Document the Claude Code/Cursor partial-install gap (naming collision with pre-existing `openspec-sync-specs` skill) as an accepted, deferred limitation -- not fixed in this change (user decision)

## 2. Author OpenSpec Change Artifacts

- [x] 2.1 Scaffold `openspec/changes/tighten-decision-workflow/` via `openspec new change`
- [x] 2.2 Write `proposal.md` (Why, What Changes, Capabilities, Impact, explicit non-goals)
- [x] 2.3 Write `design.md` (Context, Goals/Non-Goals, Decisions, Risks/Trade-offs)
- [x] 2.4 Write delta specs for all six capabilities under `specs/*/spec.md`
- [x] 2.5 Run `openspec validate tighten-decision-workflow`; confirm valid

## 3. Add Closed-Decision Language and Architecture-as-Shared-Memory Rules (docs/base-standards.md)

- [x] 3.1 Append new §10 "Closed-Decision Language (No Vague Qualifiers)" with the banned-phrase list and the literal grep check
- [x] 3.2 Append new §11 "Architecture as Shared Memory" pointing at `docs/architecture.md`, referencing the new skill
- [x] 3.3 Add one new bullet under existing §3 for `docs/architecture.md` (no renumbering of existing §1-9)

## 4. Fix Vague Language and Add Architecture Step (docs/openspec-tasks-mandatory-steps.md)

- [x] 4.1 Reword the three "MANDATORY if applicable" instances (Step N+3, Verification Checklist, Example Structure) into a closed applies/does-not-apply rule
- [x] 4.2 Require `tasks.md` to state "Not Applicable" explicitly (with reason) for any step that doesn't apply, rather than omitting it
- [x] 4.3 Insert new mandatory "Update Architecture Doc" step after E2E testing and before "Update Technical Documentation" (renumber current N+4 -> N+5)
- [x] 4.4 Update the Verification Checklist and Example Structure to include the new step and the no-vague-language check

## 5. Create docs/architecture.md Skeleton

- [x] 5.1 Create `docs/architecture.md` with sections: Purpose, System Overview, Domain/Glossary, Capability Map, Cross-Cutting Decisions Log, Open Questions, Change Log

## 6. Create update-architecture-doc Skill (RED -> GREEN -> REFACTOR per writing-skills)

- [x] 6.1 RED: run a pressure scenario via subagent without the skill (3 dimensions tested: direct pressure, checklist-omission, structural-consistency); document baseline verbatim -- no violation surfaced, honestly reported as such rather than forced
- [x] 6.2 GREEN: write the minimal `ai-specs/skills/update-architecture-doc/SKILL.md`; re-run the scenario with the skill; verify compliance -- PASS
- [x] 6.3 REFACTOR: no new rationalization surfaced; no further changes needed
- [x] 6.4 Run `sync-agent-symlinks` to create `.claude/skills/update-architecture-doc` and `.cursor/skills/update-architecture-doc` symlinks
- [x] 6.5 Save the RED/GREEN/REFACTOR report to `reports/2026-07-30-step-6-skill-pressure-test-update-architecture-doc.md`

## 7. Tighten enrich-us Skill (RED -> GREEN -> REFACTOR)

- [x] 7.1 RED: combined-pressure scenario (deadline + sunk-cost + authority) against a genuine blocking gap (unidentified target system); baseline finalized output anyway, admitting the rationalization "the skill doesn't technically force me to block"
- [x] 7.2 GREEN: added the architecture-doc-read step, code-consistency check, blocking-vs-non-blocking gap classification, and the new Original/Scope/Closed Decisions/Expected Behavior output format; re-tested -- PASS, agent stopped and asked instead of finalizing
- [x] 7.3 REFACTOR: no new rationalization surfaced; no further changes needed
- [x] 7.4 Save the report to `reports/2026-07-30-step-7-skill-pressure-test-enrich-us.md`

## 8. Tighten adversarial-review Skill (RED -> GREEN -> REFACTOR)

- [x] 8.1 RED: authority-pressure scenario (implementer claims a Blocker was fixed, no diff supplied); baseline correctly refused to confirm but admitted the skill has no explicit rule requiring this
- [x] 8.2 GREEN: added Step 6 (loop back to implementer until no Blocker/Major remains, explicit "a claim is not evidence" rule) and the "### Next Action" output line; re-tested -- PASS, agent quoted the new rule verbatim
- [x] 8.3 REFACTOR: no new rationalization surfaced; no further changes needed
- [x] 8.4 Save the report to `reports/2026-07-30-step-8-skill-pressure-test-adversarial-review.md`

## 9. Tighten commit Skill (RED -> GREEN -> REFACTOR)

- [x] 9.1 RED: "keep it short" pressure scenario; baseline included full detail anyway but confirmed it was judgment, not a requirement
- [x] 9.2 GREEN: made the five PR-description sections mandatory (per-layer summary, tests by type with report paths, evaluations applied, change-folder link, follow-ups); re-tested with an escalated "skip the adversarial-review section" request -- PASS, agent kept it and flagged the trade-off per the new rule
- [x] 9.3 REFACTOR: no new rationalization surfaced; also fixed a pre-existing "if applicable" instance found in the same file
- [x] 9.4 Save the report to `reports/2026-07-30-step-9-skill-pressure-test-commit.md`

## 10. Testing and Verification (MANDATORY - applicability stated explicitly per closed-decision-language)

- [x] 10.1 Unit Tests and Database State Verification: **Not Applicable** -- this change contains no application code or database; verification is via the per-skill RED/GREEN/REFACTOR reports in groups 6-9 instead
- [x] 10.2 Manual Endpoint Testing with curl: **Not Applicable** -- no backend endpoints exist in this change
- [x] 10.3 E2E Testing with Playwright MCP: **Not Applicable** -- no frontend/browser-facing workflow exists in this change
- [x] 10.4 Ran the banned-phrase grep across every file touched in this change; all matches reviewed and confirmed legitimate (quotes of the banned list itself, historical references to the one instance that was fixed, or operational conditionals like "start server if needed" that don't hide a spec decision)

## 11. Update Technical Documentation (MANDATORY)

- [x] 11.1 Synced `README.md`: Quick Start (OpenSpec version note), "Useful Skills" list (+ `update-architecture-doc`, updated `enrich-us`/`commit`/`adversarial-review` descriptions), numbered "Specific Standards" list (+ §10/§11), repo structure diagram (+ `architecture.md`), and fixed the `config.yml` / `rules: _global` example to use the real `openspec/config.yaml` filename and real artifact IDs

## 12. Update Architecture Doc (MANDATORY per this change's own new rule -- dogfooding it)

- [x] 12.1 Populated `docs/architecture.md`'s Capability Map and Change Log with this change's outcome, using the new `update-architecture-doc` skill

## 13. Verify, Archive, Commit

- [x] 13.1 Ran `openspec validate tighten-decision-workflow`; confirmed valid throughout
- [x] 13.2 Ran a real `adversarial-review` pass over the whole change, dogfooding its own new loop-back mechanism; see `reports/2026-07-30-step-13-adversarial-review-tighten-decision-workflow.md`
- [x] 13.3 Ran `openspec archive tighten-decision-workflow`
- [x] 13.4 Ran `commit` skill to produce the detailed PR description; pushing/opening the PR held for explicit user confirmation per this repo's own Git Workflow rule (`docs/base-standards.md` §9)
