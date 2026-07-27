---
description: Start the pipeline sequence to analyze a feature.
---

When the user types `/analyze <feature> <start>`, orchestrate the analysis process strictly using `.gemini/agents/`,
`.gemini/skills/` and received parameters.

# Execute in sequence

1. Analyze business logic using:
   - The agent: [analyst.md](../agents/analyst.md).
   - The skills:
     - [SKILL.md](../skills/vue/SKILL.md).
     - [SKILL.md](../skills/analyze_business_logic/SKILL.md).
     - [SKILL.md](../skills/analyze_vue_architecture/SKILL.md).
   - The parameters: `<feature>`, `<start>`.
   - The task: Analyzes a specific codebase feature across all architectural layers and generates structured functional
     and technical documentation with diagrams.
