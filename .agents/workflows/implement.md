---
description: Start the Autonomous AI Developer Pipeline sequence to implement a feature
---

When the user types `/implement <feature> <specification> <plan>`, orchestrate the implementation process
strictly using `.gemini/agents/`, `.gemini/skills/` and received parameters.

### Execution Sequence:

1. Implement code modifications using:
    - The agent: [developer.md](../agents/developer.md).
    - The skills: [SKILL.md](../skills/implementation/SKILL.md).
    - The parameters: `<feature>` and `<plan>`.
    - The task: Implement the production code for the provided `<feature>`, following the provided `<plan>` in
      context.

2. Audit implementation using:
    - The agent: [qa.md](../agents/qa.md).
    - The skills: [SKILL.md](../skills/audit_quality/SKILL.md) .
    - The parameters: `<plan>` and `<specification>`.
    - The task: Review the code in accordance with the provided implementation plan `<plan>` and `<specification>`.
      Identify any functional discrepancies, architectural inconsistencies, logical errors, or dependency concerns, and
      provide documented recommendations for resolution.
