---
description: This document contains all development rules and guidelines for this project, applicable to all AI agents (Claude, Cursor, Codex, Gemini, etc.).
alwaysApply: true
---

## 1. Core Principles

- **Small tasks, one at a time**: Always work in baby steps, one at a time. Never go forward more than one step.
- **Test-Driven Development**: Start with failing tests for any new functionality (TDD), according to the task details.
- **Type Safety**: All code must be fully typed.
- **Clear Naming**: Use clear, descriptive names for all variables and functions.
- **Incremental Changes**: Prefer incremental, focused changes over large, complex modifications.
- **Question Assumptions**: Always question assumptions and inferences.
- **Pattern Detection**: Detect and highlight repeated code patterns.

## 2. Language Standards
- **English Only**: All technical artifacts must always use English, including:
    - Code (variables, functions, classes, comments, error messages, log messages)
    - Documentation (README, guides, API docs)
    - Jira tickets (titles, descriptions, comments)
    - Data schemas and database names
    - Configuration files and scripts
    - Git commit messages
    - Test names and descriptions

## 3. Specific standards

For detailed standards and guidelines specific to different areas of the project, refer to:

- [Backend Standards](./backend-standards.md) - API development, database patterns, testing, security and backend best practices
- [Frontend Standards](./frontend-standards.md) - React components, UI/UX guidelines, and frontend architecture
- [Documentation Standards](./documentation-standards.md) - Technical documentation structure, formatting, and maintenance guidelines, including AI standards like this document
- [OpenSpec Tasks Mandatory Steps](./openspec-tasks-mandatory-steps.md) - Required checklist and execution rules when creating or updating OpenSpec `tasks.md` files
- [Architecture](./architecture.md) - Living map of system pieces, domain concepts, and cross-cutting decisions, updated with every change (see §11)

## 4. Project Skills

- Skills live in `ai-specs/skills`.
- When a request matches a skill, load and follow the corresponding `SKILL.md` automatically before continuing.
- Also load any referenced files in the skill folder (for example, `references/*.md`) when the skill requires them.

## 5. Planning Model Requirement

Planning workflows must run with Opus high reasoning.

This requirement applies to:
- `enrich-us`
- `openspec-ff-change`
- `openspec-continue-change`

Before starting any of these workflows, verify the session is using Opus high reasoning. If it is not, **self-correct** by adding `"model": "claude-opus-4-7"` to `.claude/settings.json` (use the `update-config` skill or edit directly), then continue — do not stop and ask the user. Do the same to come back to sonnet medium for any other step.

## 6. Symlink Integrity and Multi-Agent Portability

- **Canonical Source**: Keep reusable artifacts in `ai-specs` as the canonical source. Agent-specific paths (such as `.claude` and `.cursor`) should reference them through symlinks when possible.
- **Update Safety**: Whenever a file is renamed, moved, or its suffix changes, verify and update all symlinks that target it before considering the change complete.
- **New Artifact Linking**: Whenever creating a new artifact that requires multi-agent exposure (for example new agents or skills in `ai-specs`), create the corresponding symlinks from the expected agent-specific reference paths.
- **External Customization Review**: Whenever customization is introduced outside `ai-specs`, evaluate whether it should be moved into `ai-specs` and replaced with symlinks from the original locations.
- **Completion Gate**: A change is incomplete if it leaves broken symlinks, stale targets, or duplicated canonical artifacts across agent-specific folders.

## 7. Mandatory OpenSpec Artifact Updates for Post-Apply Changes

When a new fix/change request appears after `opsx:apply` (or `/apply`) and before `opsx:archive` (or `/archive`), agents must treat it as a spec update first, not as an informal "fix this quickly". It's the core principle of openspec, documentation is the source of truth.

Required order:

1. Update the current OpenSpec change artifacts that are affected (for example: scenarios, requirements/specs, and `tasks.md`). Don't add tasks as "bugfixes" but as part of the initial design, thus in the proper section
2. If artifact regeneration is needed, run the corresponding OpenSpec step (`opsx:continue`, `opsx:ff`, or equivalent) before coding.
3. Implement code only after artifacts reflect the new request.
4. Re-run verification against the updated artifacts before archiving.

Do not apply direct code-only fixes in this window without updating OpenSpec artifacts.

## 8. Default to the OpenSpec Workflow for Feature/Behavior Changes (Standing Rule)

**This is a standing instruction — follow it automatically, without being asked each time.** The user should not need to type "/opsx:propose" or explain the OpenSpec process in every message. When a request in this project is a real feature or behavior change, start the OpenSpec workflow (proposal → design → specs → tasks, then apply → verify → archive) on your own initiative before implementing.

**Use the OpenSpec workflow (do this automatically) when the request:**
- Adds a new capability or changes an existing one
- Changes a documented business/behavior rule
- Changes the structure or format of a deliverable
- Would otherwise leave `openspec/specs/` out of sync with what the system actually does

**Skip the OpenSpec workflow (just make the edit directly) when the request is:**
- A typo fix, wording tweak, or other trivial, non-behavioral correction
- A pure documentation clarification that doesn't change what the system does
- Explicitly framed as "just a quick fix," and the user isn't asking for a tracked change

**If it's ambiguous which category a request falls into, ask the user in one sentence** (e.g. "Is this a full feature — should I run it through the OpenSpec proposal process — or just a quick fix?") rather than silently guessing either way.

**Each distinct feature/change still gets its own OpenSpec change folder.** This standing rule means you don't have to be told to use OpenSpec each time — it does not mean multiple unrelated changes get bundled into one proposal.

## 9. Git Workflow

### Every Session
- **At the start**: report the current branch and any uncommitted changes before doing anything else.
- **At the end**: summarize what changed, then ask:
    1. Should these changes be merged into main?
    2. Should this be pushed to GitHub?

### Branches
- Always create a new branch before making changes. Never commit directly to main.
- For everyday/ad-hoc work, name branches after the task, e.g. `fix-invoice-report`, `update-checklist`.
- **Does not override OpenSpec/frontend conventions**: this plain-naming rule applies to everyday, ad-hoc work only. Changes going through the formal OpenSpec process still use the branch naming defined in [OpenSpec Tasks Mandatory Steps](./openspec-tasks-mandatory-steps.md) (`feature/[change-name]`, with a `-backend`/`-frontend` suffix per [Frontend Standards](./frontend-standards.md)).
- Only merge into main after explicit confirmation that the result is approved.
- Ask before deleting a branch, even if it's already merged.

### Commits
- Use clear, plain-language commit messages: what changed and why.
- Keep unrelated changes in separate commits.

### Safety
- Before `reset --hard`, force-pushing, or deleting anything, explain what will happen in plain language and wait for confirmation.
- Never force-push.

### .gitignore
- Maintain a `.gitignore` for this project. Suggest additions whenever noticing: temporary or system files (`.DS_Store`, `Thumbs.db`), credentials or API keys (`.env`), and any real client or billing data.
- Never commit files containing client data, passwords, or API keys.

## 10. Closed-Decision Language (No Vague Qualifiers)

- **Banned phrases**: OpenSpec artifacts (proposals, specs, designs, tasks) and skill outputs must not contain vague, ambiguity-preserving qualifiers: "if applicable", "if needed", "preferred", "maybe", "possibly", "as appropriate", "when necessary", or "TBD" without an attached owner and date.
- **Resolve, don't soften**: any instance found must become either an explicit decision, or an explicit "Open Question" entry naming what must be resolved and by whom. Never leave it as a silent soft qualifier.
- **Mechanical check**: before finalizing or archiving an OpenSpec change, run `grep -inE 'if applicable|if needed|preferred|maybe|possibly|as appropriate' <file>` (or equivalent) against its artifacts and resolve any match.

## 11. Architecture as Shared Memory

- **Living document**: `docs/architecture.md` is a continuously-maintained document covering Purpose, System Overview, Domain/Glossary, Capability Map, Cross-Cutting Decisions Log, Open Questions, and a Change Log.
- **Update on every change**: `docs/architecture.md` must be updated (Capability Map and Change Log at minimum) before any OpenSpec change is archived, using the `update-architecture-doc` skill rather than ad hoc edits.
- **Why**: this gives any new agent session shared context about where the pieces of the system live without needing to re-read the whole codebase.
