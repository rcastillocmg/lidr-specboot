## Purpose
Ensures an independent adversarial review not only finds problems but forces them to actually get fixed before a change can be archived, by looping back into implementation until nothing serious remains open.

## ADDED Requirements

### Requirement: Loop back to implementer on serious findings
The `adversarial-review` skill SHALL, when its verdict is `FAIL` or includes any Blocker or Major finding, hand off the findings table and instruct re-entry into `/apply`, then re-run the adversarial review on the updated diff, repeating until the verdict is `PASS` or `PASS WITH GAPS` with only Minor findings remaining.

#### Scenario: Review finds a Blocker
- **WHEN** an adversarial review produces a verdict of `FAIL` or a Blocker/Major finding
- **THEN** the agent MUST NOT recommend archiving; it MUST hand the findings to the implementer, wait for the fix, and re-run the adversarial review before any archive recommendation

### Requirement: Explicit next-action signal
The `adversarial-review` skill SHALL append a "### Next Action" line to its output stating either `RE-ENTER /apply` or `READY FOR /archive`.

#### Scenario: Review completes
- **WHEN** an adversarial review finishes and produces a verdict
- **THEN** the output MUST include a "### Next Action" line consistent with that verdict
