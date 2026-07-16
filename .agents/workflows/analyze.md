---
description: Start the pipeline sequence to analyze a feature.
---

When the user types `/analyze <feature> <class> <method>`, orchestrate the analysis process strictly using
`.gemini/agents/`, `.gemini/skills/` and received parameters.

# Execute in sequence

1. Analyze persistence using:
    - The agent: [analyst.md](../agents/analyst.md).
    - The skills: [SKILL.md](../skills/analyze_persistence/SKILL.md).
    - The parameters: `<feature>`, `<class>` and `<method>`.
    - The task: Analyze the provided `<feature>` starting at provided `<class>` and `<method>` to identify all Data
      Repositories and Entities invoked and generate a documentation file that maps out exactly which database tables
      and columns are accessed.
2. Analyze business logic using:
    - The agent: [analyst.md](../agents/analyst.md).
    - The skills: [SKILL.md](../skills/analyze_business_logic/SKILL.md).
    - The parameters: `<feature>`, `<class>` and `<method>`.
    - The task: Analyzes a specific codebase feature across all architectural layers and generates structured functional
      documentation with diagrams.
3. Analyze architecture using:
    - The agent: [analyst.md](../agents/analyst.md).
    - The skills: [SKILL.md](../skills/analyze_architecture/SKILL.md).
    - The parameters: `<feature>`, `<class>` and `<method>`.
    - The task: Analyzes a specific codebase feature across all architectural layers and generates structured technical
      documentation with diagrams.
