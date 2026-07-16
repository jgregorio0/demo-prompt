---
name: Audit quality
description: Review the code in accordance with the provided implementation plan `<plan>` and `<specification>`. Identify any functional discrepancies, architectural inconsistencies, logical errors, or dependency concerns, and provide documented recommendations for resolution.
---

# Context

- **Source Directory:** The raw codebase is located in `src/`.
- **Coding Rules:** Code quality rules are defined in `AGENTS.md`.

---

# Critical Constraints

1. **Strict Realization:** Do NOT replace complex functions with placeholders, comments like `// TODO: implement later`,
   or dummy logs. All logic must be complete and production-ready.
2. **File Preservation:** Maintain the existing folder structure inside `src/`.

---

# Steps to follow

1. **Assess Alignment**
    - Open and read the `[specification]`.
    - Map every requirement (database schema, API endpoints, state management, UI components) to the files in `src/`.
    - Identify any feature gaps or architectural deviations. If a feature is missing or implemented incorrectly compared
      to the spec, flag it for immediate correction.

2. **Bug Hunting**
    - **Dependency Check**: Audit `build.gradle` or equivalent configuration files. Ensure versions are compatible and
      that there are no missing packages import-referenced in the code.
    - **Error Tracing**: Review all asynchronous operations, file I/O, database queries, and external API requests.
      Ensure every operation uses robust try-catch blocks or error-handling middleware.
    - **Logic Verification**: Step through complex loops, conditional branches, and state modifications to find logic
      breaks, boundary condition errors, and memory leaks.

3. **Documentation**
    - Document the identified issues including recommendations for resolution.

---

# Output checklist

- [ ] The identified issues including recommendations for resolution are documented on `docs/[feature]/qa.md`.
