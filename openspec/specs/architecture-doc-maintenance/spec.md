# architecture-doc-maintenance Specification

## Purpose
Maintains a single, continuously-updated architecture document that gives any new agent session shared context about where the pieces of a project live, without needing to re-read the whole codebase.
## Requirements
### Requirement: Living architecture document
The system SHALL maintain `docs/architecture.md` (or the project's equivalent path) as a living document covering Purpose, System Overview, Domain/Glossary, Capability Map, Cross-Cutting Decisions Log, Open Questions, and a Change Log.

#### Scenario: A new OpenSpec change is archived
- **WHEN** an OpenSpec change is archived
- **THEN** `docs/architecture.md`'s Capability Map and Change Log MUST be updated to reflect the change before the archive step is considered complete

### Requirement: Dedicated update skill
The system SHALL provide a skill (`update-architecture-doc`) whose sole job is to keep `docs/architecture.md` current, invoked as a mandatory step during task execution.

#### Scenario: Task execution reaches the architecture-update step
- **WHEN** an agent executing a change's `tasks.md` reaches the mandatory "Update Architecture Doc" step
- **THEN** the agent MUST invoke the `update-architecture-doc` skill rather than editing `docs/architecture.md` ad hoc, and MUST NOT mark the step done under "docs can wait" or similar reasoning

