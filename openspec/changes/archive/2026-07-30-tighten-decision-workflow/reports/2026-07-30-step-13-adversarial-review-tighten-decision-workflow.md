## Adversarial review

**Scope**: tighten-decision-workflow (whole change)
**Sources**: `openspec/changes/tighten-decision-workflow/{proposal.md,design.md,specs/*/spec.md,tasks.md}` + `git diff` against the working tree (6 modified tracked files: `README.md`, `ai-specs/skills/adversarial-review/SKILL.md`, `ai-specs/skills/commit/SKILL.md`, `ai-specs/skills/enrich-us/SKILL.md`, `docs/base-standards.md`, `docs/openspec-tasks-mandatory-steps.md`) plus new files (`docs/architecture.md`, `ai-specs/skills/update-architecture-doc/SKILL.md`, `openspec/` tree).

### Spec and task alignment

Checked all six capability specs' Requirements/Scenarios against the actual diffs. All six are satisfied by the corresponding edits (base-standards.md §10/§11, openspec-tasks-mandatory-steps.md rewording + new step, docs/architecture.md, update-architecture-doc skill, enrich-us edits, adversarial-review edits, commit edits).

### Findings

| Severity | Area | Finding | Evidence | Suggested fix (code / spec / tests) | Status |
|----------|------|---------|----------|--------------------------------------|--------|
| Blocker | `enrich-us` composition risk | New Step 6 (blocking-gap handling, ask before finalizing) and existing Step 9 (Jira write-back, advances ticket status) could fire together: an agent could ask a blocking clarifying question per Step 6 but Step 9 had no guard preventing a write-back / status advance to "Pending refinement validation" while that question was still unanswered. | `ai-specs/skills/enrich-us/SKILL.md`, old step 9 text had no conditional on step 6's outcome. | Fixed: step 9 now explicitly gated on "no unresolved blocking gap"; if blocking, no Jira write-back or status change happens. | **Fixed during this review** |
| Blocker | `commit` cross-cutting inconsistency | New mandatory "Evaluations applied" PR section (item 3) implicitly assumed every change went through the OpenSpec workflow (requiring an adversarial-review verdict and Verification Checklist outcome), but `docs/base-standards.md` §8 explicitly allows quick fixes to skip OpenSpec entirely -- as written, a trivial typo-fix commit would have an unsatisfiable mandatory PR section. | `ai-specs/skills/commit/SKILL.md`, item 3 before fix. | Fixed: item 3 now branches on whether the change went through OpenSpec; if not, requires an explicit "No OpenSpec change -- quick fix" statement instead. | **Fixed during this review** |
| Minor | `openspec-tasks-mandatory-steps.md` Verification Checklist scope | The new "no banned vague-qualifier phrases" checklist line only mentions checking `tasks.md`, while `docs/base-standards.md` §10's mechanical check applies to all OpenSpec artifacts (proposal/design/specs/tasks). | Reviewed; this is a reasonable scoping choice since this file is specifically about `tasks.md` authoring -- the broader multi-artifact check is `docs/base-standards.md` §10's responsibility and was performed manually for this change (task 10.4). | No fix required; noted as reviewed, not a defect. | Reviewed, no action |
| Question / assumption | Claude Code / Cursor partial OpenSpec install | 3 of 6 OpenSpec built-in skills and no command shortcuts installed for these two tools, due to a naming collision with this repo's pre-existing `openspec-sync-specs` skill. | `openspec init` output during Task Group 1; documented in `docs/architecture.md` Cross-Cutting Decisions Log and Open Questions. | Accepted, deferred gap per explicit user decision -- not a defect in this change. | Accepted (user decision) |

### Verdict
**PASS** — both Blockers found during this review were fixed on the spot and re-verified (banned-phrase re-scan clean, file contents re-read to confirm the fix). No Blockers or Majors remain open.

### Recommended next steps (before archive)
- None blocking. Proceed to archive.

### Next Action
READY FOR /archive
