---
description: Start the pipeline sequence to design a feature.
---

When the user types `/design <feature> <analysis> <functional>`, orchestrate the design process strictly using
`.gemini/agents/`, `.gemini/skills/` and received parameters.

# Execution Sequence:

1. Create a specification document in the `<specification>` file using:
   - The agent: [product_manager.md](../agents/product_manager.md).
   - The skills: [SKILL.md](../skills/design_specification/SKILL.md).
   - The parameters: `<feature>`, `<analysis>` and `<functional>`.
   - The task: Generate a specification document for a provided <feature> using the provided <analysis>
     and <functional>.
2. Create an implementation plan document using:
   - The agent: [architect.md](../agents/architect.md).
   - The skills:
     - [SKILL.md](../skills/vue/SKILL.md).
     - [SKILL.md](../skills/unit-testing/SKILL.md).
     - [SKILL.md](../skills/design_implementation_plan/SKILL.md).
   - The parameters:
     - `<specification>` generated in the previous step.
     - `<feature>` and `<analysis>`.
   - The task: Creates a detailed implementation plan for a given feature <specification>. To be used for planning the
     implementation of feature specifications.
