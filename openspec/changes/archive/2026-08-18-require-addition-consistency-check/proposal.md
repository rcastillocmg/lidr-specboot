## Why

While planning `add-ai-governance-copilot-default-skill`, a manual, ad hoc consistency check (naming collisions, trigger overlap, `writing-skills` format compliance, wording precedent in `docs/base-standards.md`) caught a real conflict -- the copied skill's frontmatter violated this repo's own skill-writing rules -- that none of the existing mandatory steps in `docs/openspec-tasks-mandatory-steps.md` would have caught, because nothing currently requires checking a new addition against what this repo has already built. Without a standing rule, catching a conflict depends on someone remembering to ask for the check, as happened here only because it was requested directly.

## What Changes

- Add a new standing rule to `docs/base-standards.md`: before any addition to this project (a skill, an agent, an OpenSpec capability, a documentation section, a symlink, or any other artifact under `ai-specs/`, `docs/`, `.claude/`, `.cursor/`, or `openspec/`) is finalized, it MUST be checked against what already exists in this repo, and any conflict found MUST be resolved or explicitly flagged -- never silently merged.
- Define what counts as a conflict (naming collision, overlapping trigger/scope, contradictory wording in governance docs, non-compliance with this repo's own format rules for that artifact type) and where to check (`ai-specs/skills/*`, `ai-specs/agents/*`, `openspec/specs/*`, `docs/base-standards.md` and the docs it links to, `docs/architecture.md`'s Capability Map).
- Add a new mandatory step to `docs/openspec-tasks-mandatory-steps.md` -- "Consistency Check Against Existing Artifacts" -- positioned before "Update Architecture Doc," so every future `tasks.md` in this repo includes and executes this check, the same way "Update Architecture Doc" became mandatory for every change.

## Capabilities

### New Capabilities
- `addition-consistency-check`: the standing rule in `docs/base-standards.md` requiring every addition to this project to be checked against existing artifacts for conflicts before it is finalized, with a defined conflict taxonomy and check locations.

### Modified Capabilities
- `openspec-task-planning`: `docs/openspec-tasks-mandatory-steps.md` gains a new mandatory "Consistency Check Against Existing Artifacts" step (never marked Not Applicable, mirroring how "Update Architecture Doc" is never Not Applicable), and the Verification Checklist confirms its presence.

## Impact

- Files: `docs/base-standards.md` (new section), `docs/openspec-tasks-mandatory-steps.md` (new mandatory step + updated example structure + updated Verification Checklist), `docs/architecture.md`.
- Explicitly out of scope: retroactively re-checking artifacts already merged before this rule exists (including the in-flight `add-ai-governance-copilot-default-skill` change, which already performed this check manually and is not reopened by this change); `packages/specboot/template/` (already diverged from this repo independently, per existing project convention); building any automated/mechanical conflict scanner -- unlike the closed-decision-language grep check (`docs/base-standards.md` §10), "conflict with what's already built" is not fully mechanizable, so this rule defines a checklist for the agent to apply, not a script to run.
