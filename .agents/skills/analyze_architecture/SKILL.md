---
name: Analyze architecture
description: "Analyzes a specific codebase feature across all architectural layers and generates structured technical documentation with diagrams."
---

# Context

- The feature exposed via a set of API endpoints.
- You have access to the codebase containing the routing, controller, application/service, domain/business logic, and
  data persistence layers in the code directories: [code_directories.md](code_directories.md).

# Steps to follow

1. **API Entry Points:** Identify the HTTP methods, routes, and controller classes handling rocket-related requests.
2. **Business Logic & Domain:** Trace how data is processed, validated, and manipulated within the domain layers.
3. **Data & Persistence:** Identify the underlying database models, relationships, and third-party API integrations
   involved.

# Specification Template

Generate the final documentation exactly matching the structural template [template.md](template.md).

## Constraints
- Do not run tests
- Do not modify any existing file.
- Write the output file only.

## Output checklist

- [ ] The output should be a md file at `docs/[feature]/architecture.md` containing:
- [ ] Title
- [ ] Description
- [ ] Flow chart
- [ ] Sequence diagramle
- [ ] Class diagramle
- [ ] Entity relationship diagram
