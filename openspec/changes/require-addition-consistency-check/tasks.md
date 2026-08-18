## 0. Setup: Create Feature Branch (MANDATORY - FIRST STEP)

- [ ] 0.1 Create feature branch `feature/require-addition-consistency-check` from main
- [ ] 0.2 Verify branch creation and current branch status

## 1. Consistency Check Against Existing Artifacts (MANDATORY)

- [ ] 1.1 Check the new `addition-consistency-check` capability name against existing capability names in `openspec/specs/*` for collisions -- none found (`closed-decision-language`, `architecture-doc-maintenance`, `user-story-enrichment`, `adversarial-review`, `commit-pr-summary`, `openspec-task-planning`)
- [ ] 1.2 Check the new `docs/base-standards.md` section number/heading against existing sections (currently through §12, since `add-ai-governance-copilot-default-skill` has not yet added a section) to avoid a numbering collision at implementation time
- [ ] 1.3 Confirm the new mandatory step's position (before "Update Architecture Doc") does not conflict with the existing step ordering in `docs/openspec-tasks-mandatory-steps.md`

## 2. Add the Standing Rule to Base Standards

- [ ] 2.1 Add a new section to `docs/base-standards.md` stating the standing rule: every addition to this project must be checked against what already exists before being finalized
- [ ] 2.2 Include the conflict taxonomy (naming collision, overlapping trigger/scope, contradictory wording, convention non-compliance) and the check locations (`ai-specs/skills/*`, `ai-specs/agents/*`, `openspec/specs/*`, `docs/base-standards.md` and linked docs, `docs/architecture.md`'s Capability Map)
- [ ] 2.3 State the resolve-or-flag requirement, cross-referencing §10 (Closed-Decision Language) for how an unresolved conflict must be recorded as an explicit Open Question rather than left silent

## 3. Add the Mandatory Step to the Task-Planning Rules

- [ ] 3.1 Add a "Consistency Check Against Existing Artifacts" step to `docs/openspec-tasks-mandatory-steps.md`, positioned before "Update Architecture Doc," marked as never Not Applicable
- [ ] 3.2 Update the Example Structure section to show the new step in its correct position
- [ ] 3.3 Update the Verification Checklist to include confirming this step's presence

## 4. Review and Update Existing Unit Tests (MANDATORY) - Not Applicable

- [ ] 4.1 Not Applicable: this change adds documentation sections only; there is no application code or existing unit test suite in this repo for this change to touch (per `openspec/config.yaml`, this repo has no backend/frontend application codebase)

## 5. Run Unit Tests and Verify Database State (MANDATORY) - Not Applicable

- [ ] 5.1 Not Applicable: no unit test suite or database exists in this repo

## 6. Manual Endpoint Testing with curl (MANDATORY) - Not Applicable

- [ ] 6.1 Not Applicable: no backend endpoints exist in this repo

## 7. E2E Testing with Playwright MCP (MANDATORY - see Applicability Rule) - Not Applicable

- [ ] 7.1 Not Applicable: no frontend/browser-facing surface in this change

## 8. Update Architecture Doc (MANDATORY)

- [ ] 8.1 Invoke the `update-architecture-doc` skill (do not edit `docs/architecture.md` ad hoc)
- [ ] 8.2 Add `addition-consistency-check` to the Capability Map, and note the `openspec-task-planning` modification
- [ ] 8.3 Append a Change Log entry naming this change
- [ ] 8.4 Record the two-capability split decision (see design.md - Decisions) as a Cross-Cutting Decision if not already reflected

## 9. Update Technical Documentation (MANDATORY)

- [ ] 9.1 Update `README.md`'s Core Development Rules list to mention the new standing rule and its `docs/base-standards.md` section, following the existing pattern for §8-§11
- [ ] 9.2 Re-read `docs/openspec-tasks-mandatory-steps.md` end-to-end to confirm the new step reads consistently with the rest of the document (numbering, "When This Applies" section, Failure to Follow section)

## 10. Verification

- [ ] 10.1 Run `grep -inE 'if applicable|if needed|preferred|maybe|possibly|as appropriate' <file>` against every new/changed artifact in this change (`proposal.md`, `design.md`, both spec deltas, `tasks.md`, the `docs/base-standards.md` and `docs/openspec-tasks-mandatory-steps.md` additions) and resolve any match, per `docs/base-standards.md` §10
- [ ] 10.2 Run `openspec validate require-addition-consistency-check --strict` and resolve any reported errors
- [ ] 10.3 Confirm this change's own artifacts satisfy the rule it introduces (self-check: does this change conflict with anything already built? -- see Section 1 above)
