## 0. Setup: Create Feature Branch (MANDATORY - FIRST STEP)

- [x] 0.1 Create feature branch `feature/add-ai-governance-copilot-default-skill` from main
- [x] 0.2 Verify branch creation and current branch status

## 1. Add Project-Local Skill Copy

- [x] 1.1 Read the canonical skill file at `CMG_AI_Governance/_local-claude-skills/cmg-ai-governance-copilot/SKILL.md`
- [x] 1.2 Create `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md` with the body (every operational instruction, all seven steps, the hard boundaries) identical to the canonical source and verify it matches byte-for-byte
- [x] 1.3 Replace the frontmatter `description` field with a trigger-only rewrite compliant with `ai-specs/skills/writing-skills/SKILL.md` (starts with "Use when...", states only triggering conditions, no workflow summary); leave the `name` field and the body untouched
- [x] 1.4 Verify total frontmatter length (`name` + `description` block) is 1024 characters or fewer, per `writing-skills`' hard cap

## 2. Expose Skill to Claude and Cursor

- [x] 2.1 Invoke the `sync-agent-symlinks` skill to create `.claude/skills/cmg-ai-governance-copilot` and `.cursor/skills/cmg-ai-governance-copilot` as symlinks to `ai-specs/skills/cmg-ai-governance-copilot`
- [x] 2.2 Verify both symlinks resolve and point at the canonical folder, with no broken or orphaned links left behind

## 3. Declare Standing Default in Base Standards

- [x] 3.1 Add a new subsection to `docs/base-standards.md` declaring `cmg-ai-governance-copilot` a standing default skill for this project, proactively triggered the same way it is embedded by default on the exo cluster platform
- [x] 3.2 State plainly in that subsection that the project-local copy is a point-in-time copy of the canonical `CMG_AI_Governance` source, and that re-syncing it after the canonical source changes is a manual step, not automatic
- [x] 3.3 State plainly in that subsection that the frontmatter `description` was intentionally reworded at copy time to comply with this repo's `writing-skills` rules, while the skill body was kept byte-for-byte identical to the canonical source

## 4. Review and Update Existing Unit Tests (MANDATORY) - Not Applicable

- [x] 4.1 Not Applicable: this change adds a skill file, two symlinks, and documentation only; there is no application code or existing unit test suite in this repo for this change to touch (per `openspec/config.yaml`, this repo has no backend/frontend application codebase)

## 5. Run Unit Tests and Verify Database State (MANDATORY) - Not Applicable

- [x] 5.1 Not Applicable: no unit test suite or database exists in this repo

## 6. Manual Endpoint Testing with curl (MANDATORY) - Not Applicable

- [x] 6.1 Not Applicable: no backend endpoints exist in this repo

## 7. E2E Testing with Playwright MCP (MANDATORY - see Applicability Rule) - Not Applicable

- [x] 7.1 Not Applicable: no frontend/browser-facing surface in this change

## 8. Update Architecture Doc (MANDATORY)

- [x] 8.1 Invoke the `update-architecture-doc` skill (do not edit `docs/architecture.md` ad hoc)
- [x] 8.2 Add `ai-governance-copilot` to the Capability Map
- [x] 8.3 Append a Change Log entry naming this change
- [x] 8.4 Record the copy-vs-symlink sourcing decision as a Cross-Cutting Decision (see design.md - Decisions)

## 9. Update Technical Documentation (MANDATORY)

- [x] 9.1 Update `README.md` to mention `cmg-ai-governance-copilot` wherever it enumerates this project's skills
- [x] 9.2 Re-read `docs/base-standards.md` §4 (Project Skills) and §6 (Symlink Integrity) to confirm the new subsection reads consistently with both

## 10. Verification

- [x] 10.1 Run `grep -inE 'if applicable|if needed|preferred|maybe|possibly|as appropriate' <file>` against every new/changed artifact in this change (`proposal.md`, `design.md`, `specs/ai-governance-copilot/spec.md`, `tasks.md`, the `docs/base-standards.md` addition) and resolve any match, per `docs/base-standards.md` §10
- [x] 10.2 Run `openspec validate add-ai-governance-copilot-default-skill --strict` and resolve any reported errors
