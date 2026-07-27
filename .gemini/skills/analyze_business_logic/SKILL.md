---
name: Analyze business logic
description: 'Analyzes a specific codebase feature across all architectural layers and generates structured functional documentation with diagrams.'
---

# Context

- Provided [feature] to analyze.
- Provided [start] code reference to start the analysis.
- You have access to the code in the directories defined by [code_directories.md](../../common/code_directories.md).

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

- Do not install dependencies.
- Do not run server.
- Do not run tests.
- Do not modify any existing file.

# Output Checklist

- [ ] Saved exactly to file path: docs/[feature]/business_logic.md
