---
name: Analyze architecture
description: 'Analyzes a specific frontend feature across UI, routing, state, and API integration layers, generating structured technical documentation with diagrams.'
---

# Context

- Provided [feature] to analyze.
- Provided [start] code reference to start the analysis.
- You have access to the code in the directories defined by [code_directories.md](../../common/code_directories.md).

# Steps to follow

1. **Routing & Views Entry Points:** Identify the Vue Router routes, dynamic path parameters, navigation guards, and
   parent Layout/View components handling the feature's entry points.
2. **Component & State Composition:** Trace how data is managed locally via Vue 3 Composition API (`ref`, `reactive`),
   custom composables (e.g., `useFeature`), and global state stores (Pinia or Vuex). Map the parent-child component
   hierarchy, including key props and emitted events (`emits`).
3. **API Integration & Data Flow:** Identify the API calls (Axios, Fetch, or SDK clients), request/response payloads,
   loading/error boundary strategies, and how external data maps down to UI states.

# Specification Template

Generate the final documentation exactly matching the structural template [template.md](template.md).

## Constraints

- Do not run development servers, linters, or test suites.
- Do not modify any existing source file.

## Output checklist

- [ ] The output should be a markdown file at `docs/[feature]/architecture.md`
