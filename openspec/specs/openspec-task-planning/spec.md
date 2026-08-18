# openspec-task-planning Specification

## Purpose
Governs how `tasks.md` files are authored for any OpenSpec change in this repo: mandatory steps, explicit applicability, and a verification checklist -- so no step is silently skipped and no applicability is left ambiguous.
## Requirements
### Requirement: Explicit applicability instead of "if applicable"
Any mandatory step in `docs/openspec-tasks-mandatory-steps.md` that depends on the kind of change being made SHALL state its applicability as a closed rule (applies when X; does not apply when Y), and `tasks.md` MUST state "Not Applicable" explicitly for any such step that does not apply, rather than omitting it silently.

#### Scenario: Authoring tasks.md for a docs-only change
- **WHEN** an agent authors `tasks.md` for a change with no application code changes (e.g. this change itself)
- **THEN** steps like unit testing, curl endpoint testing, and E2E testing MUST each appear in `tasks.md` explicitly marked "Not Applicable" with a one-line reason, rather than being left out

### Requirement: Mandatory architecture-doc-update step
`docs/openspec-tasks-mandatory-steps.md` SHALL include a mandatory "Update Architecture Doc" step positioned after E2E testing and before "Update Technical Documentation."

#### Scenario: Authoring tasks.md for any change
- **WHEN** an agent authors `tasks.md` for any change
- **THEN** the generated task list MUST include the "Update Architecture Doc" step in the correct position, and the Verification Checklist MUST confirm it is present

### Requirement: Mandatory consistency-check step
`docs/openspec-tasks-mandatory-steps.md` SHALL include a mandatory "Consistency Check Against Existing Artifacts" step, positioned before "Update Architecture Doc," and this step SHALL never be marked Not Applicable, mirroring how the "Update Architecture Doc" step is handled.

#### Scenario: Authoring tasks.md for any change
- **WHEN** an agent authors `tasks.md` for any change
- **THEN** the generated task list MUST include the "Consistency Check Against Existing Artifacts" step in the correct position, and the Verification Checklist MUST confirm it is present

