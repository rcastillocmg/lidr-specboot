## 0. Setup: Create Feature Branch (MANDATORY - FIRST STEP)

- [x] 0.1 Create feature branch `feature/require-addition-consistency-check` from main
- [x] 0.2 Verify branch creation and current branch status

## 1. Consistency Check Against Existing Artifacts (MANDATORY)

- [x] 1.1 Check the new `addition-consistency-check` capability name against existing capability names in `openspec/specs/*` for collisions -- none found (`adversarial-review`, `ai-governance-copilot`, `architecture-doc-maintenance`, `closed-decision-language`, `commit-pr-summary`, `openspec-task-planning`, `user-story-enrichment`)
- [x] 1.2 Check the new `docs/base-standards.md` section number/heading against existing sections -- **finding**: `add-ai-governance-copilot-default-skill` has since been implemented and merged, taking §12 (`CMG AI Governance Co-Pilot`). The plan's assumed number is stale; the new section in this change is **§13**, not §12.
- [x] 1.3 Confirm the new mandatory step's position (before "Update Architecture Doc") does not conflict with the existing step ordering in `docs/openspec-tasks-mandatory-steps.md` -- confirmed: Step 0, Mandatory Steps intro, Step N+1 (Unit Tests), N+2 (curl), N+3 (E2E), N+4 (Update Architecture Doc); the new step slots cleanly between N+3 and N+4

## 2. Add the Standing Rule to Base Standards

- [x] 2.1 Add a new section to `docs/base-standards.md` stating the standing rule: every addition to this project must be checked against what already exists before being finalized (landed as §13, per the finding in Section 1 above)
- [x] 2.2 Include the conflict taxonomy (naming collision, overlapping trigger/scope, contradictory wording, convention non-compliance) and the check locations (`ai-specs/skills/*`, `ai-specs/agents/*`, `openspec/specs/*`, `docs/base-standards.md` and linked docs, `docs/architecture.md`'s Capability Map)
- [x] 2.3 State the resolve-or-flag requirement, cross-referencing §10 (Closed-Decision Language) for how an unresolved conflict must be recorded as an explicit Open Question rather than left silent

## 3. Add the Mandatory Step to the Task-Planning Rules

- [x] 3.1 Add a "Consistency Check Against Existing Artifacts" step to `docs/openspec-tasks-mandatory-steps.md`, positioned before "Update Architecture Doc," marked as never Not Applicable (landed as Step N+4, renumbering Update Architecture Doc to N+5 and Update Technical Documentation to N+6)
- [x] 3.2 Update the Example Structure section to show the new step in its correct position
- [x] 3.3 Update the Verification Checklist to include confirming this step's presence

## 4. Review and Update Existing Unit Tests (MANDATORY) - Not Applicable

- [x] 4.1 Not Applicable: this change adds documentation sections only; there is no application code or existing unit test suite in this repo for this change to touch (per `openspec/config.yaml`, this repo has no backend/frontend application codebase)

## 5. Run Unit Tests and Verify Database State (MANDATORY) - Not Applicable

- [x] 5.1 Not Applicable: no unit test suite or database exists in this repo

## 6. Manual Endpoint Testing with curl (MANDATORY) - Not Applicable

- [x] 6.1 Not Applicable: no backend endpoints exist in this repo

## 7. E2E Testing with Playwright MCP (MANDATORY - see Applicability Rule) - Not Applicable

- [x] 7.1 Not Applicable: no frontend/browser-facing surface in this change

## 8. Update Architecture Doc (MANDATORY)

- [x] 8.1 Invoke the `update-architecture-doc` skill (do not edit `docs/architecture.md` ad hoc)
- [x] 8.2 Add `addition-consistency-check` to the Capability Map, and note the `openspec-task-planning` modification
- [x] 8.3 Append a Change Log entry naming this change
- [x] 8.4 Record the two-capability split decision (see design.md - Decisions) as a Cross-Cutting Decision if not already reflected

## 9. Update Technical Documentation (MANDATORY)

- [x] 9.1 Update `README.md`'s Core Development Rules list to mention the new standing rule and its `docs/base-standards.md` section, following the existing pattern for §8-§11 -- also caught and fixed a pre-existing gap: §12 (from `add-ai-governance-copilot-default-skill`) was never added to this list either, added both in the same pass
- [x] 9.2 Re-read `docs/openspec-tasks-mandatory-steps.md` end-to-end to confirm the new step reads consistently with the rest of the document (numbering, "When This Applies" section, Failure to Follow section) -- confirmed consistent, no stale step-number references found

## 10. Verification

- [x] 10.1 Run `grep -inE 'if applicable|if needed|preferred|maybe|possibly|as appropriate' <file>` against every new/changed artifact in this change (`proposal.md`, `design.md`, both spec deltas, `tasks.md`, the `docs/base-standards.md` and `docs/openspec-tasks-mandatory-steps.md` additions) and resolve any match, per `docs/base-standards.md` §10 -- all matches reviewed: pre-existing §10 rule text quoting its own banned list, and pre-existing plain-English "if/as needed" phrasing already in `openspec-tasks-mandatory-steps.md` before this change; none are new vague-qualifier violations
- [x] 10.2 Run `openspec validate require-addition-consistency-check --strict` and resolve any reported errors -- valid
- [x] 10.3 Confirm this change's own artifacts satisfy the rule it introduces (self-check: does this change conflict with anything already built? -- see Section 1 above) -- yes: the check caught and corrected the stale §12 assumption before finalizing
