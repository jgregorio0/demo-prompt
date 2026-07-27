---
name: Implementation
description: Implement the production code for the provided `<feature>`, following the provided `<plan>`.
---

# Context

- **Specification File:** A file [specification] is provided with the specification. Ask for the specification
  if not provided.
- **Implementation Plan:** A file [plan] is provided with the implementation plan. Ask for the implementation plan
  if not provided.
- **Coding Standards:** A file `AGENTS.md` is provided with common coding rules. Follow them carefully.

---

# Constraints

**Strict Adherence:** Every line of code must comply with `AGENTS.md`.

---

# Steps to follow

1. **Check Git status**:
   - Run `git status --porcelain`.
   - If the output is NOT empty, **STOP IMMEDIATELY** and report: "Error: Git working directory is not clean.".

2. **Branch Creation**
   - Run `git switch main` to start on main branch.
   - Run `git pull` to update last changes.
   - Run `git checkout -b feat/[feature]` to create a new branch.

3. **Context Assimilation**
   - Read and parse [specification] and [plan] to understand the requirements.

4. **Code Implementation**
   - Write the production code based on the [plan].

5. **Implementation Review**:
   - Execute the predefined tests and verify that all results confirm successful completion.
   - Address any anomalies identified and repeat the implementation review process up to 5 times as necessary. If
     issues persist after five iterations, halt the process and generate a report stating: "Error: Tests not passed."
   - Once all tests have successfully passed, mark the task as complete.

---

# Output checklist

- [ ] Active branch is `feat/[feature]`.
- [ ] Production code ready to be committed.
