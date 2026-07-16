---
name: Design specification
description: Generate a specification document for a provided <feature> using the provided <analysis> and <functional>.
---

# Context

- Functional and technical documentation <analysis> specifying how the system works (IT IS).
- Functional documentation <functional> specifying what changes are required (TO BE).

Ask for any additional context if needed.

# Specification Template

Follow this template for writing the specification file: [template.md](template.md)

## Steps to follow

1. Analyze documentation provided.
2. Define the problem. Clearly outline the problem with up to 3 user stories.
3. Outline the solution. Simplest approach for application, logic and infrastructure.
4. Set Acceptance Criteria. Up to 9 acceptance criteria in EARS format.

## Constraints

- Do not implement any code.
- Do not run any tests.

## Output checklist

- [ ] The output should be a md file at `docs/[feature]/spec.md`.
