# closed-decision-language Specification

## Purpose
Bans vague, ambiguity-preserving qualifiers from OpenSpec artifacts (proposals, specs, designs, tasks) and forces every open question into an explicit decision or an explicit flagged open question, so multiple agents cannot interpret the same artifact differently.
## Requirements
### Requirement: Banned vague-qualifier list
The system SHALL treat the following phrases as banned vague qualifiers in any OpenSpec artifact: "if applicable", "if needed", "preferred", "maybe", "possibly", "as appropriate", "when necessary", and "TBD" without an attached owner and date.

#### Scenario: Vague qualifier found in a draft artifact
- **WHEN** an agent is about to write or finalize a proposal, spec, design, or tasks artifact containing one of the banned phrases
- **THEN** the agent MUST replace it with an explicit decision, or replace it with an explicit "Open Question" entry that names what must be resolved and by whom

### Requirement: Mechanical check available
The system SHALL document a literal, reusable check for the banned phrases so compliance does not depend on manual re-reading alone.

#### Scenario: Verifying an artifact before archive
- **WHEN** an agent verifies an OpenSpec change before archiving it
- **THEN** the agent MUST run `grep -inE 'if applicable|if needed|preferred|maybe|possibly|as appropriate' <file>` (or equivalent) against the change's artifacts and resolve any match before proceeding

