## Context

See proposal.md - Why. This repo already has one precedent for a similar "always check X before finalizing" rule: Closed-Decision Language (`docs/base-standards.md` §10) bans vague qualifiers and gives a mechanical `grep` check. This new rule is broader and not fully mechanizable -- "does this conflict with what's already built" requires judgment (naming, scope, wording, format), not a fixed string match -- so it's defined as a checklist for the agent to apply, not a script.

The existing `openspec-task-planning` capability already governs what mandatory steps `docs/openspec-tasks-mandatory-steps.md` must contain (see its "Mandatory architecture-doc-update step" requirement, added by `tighten-decision-workflow`). This change follows that exact precedent for a new step.

## Goals / Non-Goals

**Goals:**
- Make the consistency check that just caught a real conflict (in `add-ai-governance-copilot-default-skill`) a standing, repeatable step instead of something that only happens when asked for.
- Follow the same split already established between `openspec-task-planning` (owns: the checklist requires this step, positioned correctly) and the substantive capability (owns: what the check actually means) -- matching how `architecture-doc-maintenance` and `openspec-task-planning` already divide that responsibility for the architecture-doc step.

**Non-Goals:**
- Building an automated conflict-detection script. Naming collisions are greppable; wording contradictions and scope overlap are not. This rule stays a checklist, not tooling.
- Retroactively auditing artifacts that existed before this rule.
- Changing anything in the already-in-flight `add-ai-governance-copilot-default-skill` change; that change already performed this check manually and stands on its own.

## Decisions

**Split the requirement across two capabilities, not one.** Considered putting everything (the substantive rule AND the tasks.md-checklist requirement) into a single new `addition-consistency-check` capability. Rejected because it would duplicate the `openspec-task-planning` capability's existing job of owning "what the mandatory-steps checklist contains," and would break the precedent already set by `architecture-doc-maintenance` / `openspec-task-planning`'s split. Instead: `addition-consistency-check` (new) owns the substantive rule -- what a conflict is, where to look, resolve-or-flag -- and `openspec-task-planning` (modified) gains one new requirement stating the checklist must include the step.

**Position the new step immediately before "Update Architecture Doc."** The architecture doc records what a change added; the consistency check should happen first, before that record is written, so a conflict caught late doesn't require re-touching the architecture doc a second time.

## Risks / Trade-offs

- **[Risk]** A checklist-based (not tool-based) rule can be performed superficially -- an agent could claim to have checked without genuinely searching. → **[Mitigation]** The spec requirement names the exact locations to check (`ai-specs/skills/*`, `ai-specs/agents/*`, `openspec/specs/*`, `docs/base-standards.md` and linked docs, `docs/architecture.md`'s Capability Map), so "I checked" has a concrete, verifiable meaning rather than being open to interpretation.
- **[Risk]** This adds one more mandatory step to an already-long `tasks.md` checklist, which could feel like process overhead on genuinely trivial additions. → **[Mitigation]** Unlike the testing steps (which can be marked Not Applicable), this step is intentionally never skippable -- a "conflict with what already exists" check costs little for a small addition and catches real problems for a larger one, so the same step scales down naturally rather than needing a separate lightweight path.

## Migration Plan

No rollback complexity: this only adds a new section to `docs/base-standards.md`, a new step to `docs/openspec-tasks-mandatory-steps.md`'s template/checklist, and a new/modified spec pair. Reverting means removing those additions; no other artifact depends on this rule existing.
