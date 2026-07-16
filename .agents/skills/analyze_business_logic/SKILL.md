---
name: Analyze business logic
description: "Analyzes a specific codebase feature across all architectural layers and generates structured functional documentation with diagrams."
---

# Context

- The high-level functional goal of the feature is located in: <functional>.
- You have access to the code in the directories defined by `code_directories.md`.

# Steps to Follow

1. **Isolate Core Rules:** Identify what constitutes a "valid" vs. "invalid" transaction or request. Look for `if/else`
   statements, throw/catch exceptions, and validation decorators.
2. **Map the State Machine:** Track how the primary domain object changes states (e.g., `Pending` -> `Approved` ->
   `Processed`). Identify exactly what conditions trigger or block a state change.
3. **Extract Calculations & Invariants:** Isolate math formulas, tax rules, discount engines, currency conversions, or
   hardcoded limits hidden in the code.
4. **Identify Side Effects:** Note any asynchronous events, notifications sent, external webhooks triggered, or audit
   logs written when business rules succeed.

# Specification Template

Generate the final document exactly matching the structure: [template.md](template.md).

## Constraints
- Do not run tests
- Do not modify any existing file.
- Write the output file only.

# Output Checklist

Verify your output strictly adheres to the following layout before completing the task:

    [ ] Saved exactly to file path: docs/[feature]/business_logic.md

    [ ] Contains a comprehensive "Core Business Invariants & Rules" table

    [ ] Contains a valid Mermaid stateDiagram-v2 mapping entity lifecycles

    [ ] Contains a valid Mermaid flowchart TD mapping the decision paths

    [ ] Contains a documented calculations section or explicitly states "No complex calculations present"
