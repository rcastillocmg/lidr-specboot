## ADDED Requirements

### Requirement: Mandatory consistency-check step
`docs/openspec-tasks-mandatory-steps.md` SHALL include a mandatory "Consistency Check Against Existing Artifacts" step, positioned before "Update Architecture Doc," and this step SHALL never be marked Not Applicable, mirroring how the "Update Architecture Doc" step is handled.

#### Scenario: Authoring tasks.md for any change
- **WHEN** an agent authors `tasks.md` for any change
- **THEN** the generated task list MUST include the "Consistency Check Against Existing Artifacts" step in the correct position, and the Verification Checklist MUST confirm it is present
