---
name: unit-testing
description: Expertise in creating comprehensive, well-structured unit and integration tests for Vue components and JavaScript modules using Vitest and @vue/test-utils. Use when the user requests to create unit test, implement unit test, escribir test para componente, añadir pruebas unitarias, or test de integración vue.
---

## Rules

### Framework

- Must use Vitest with BDD-style syntax (`describe`, `it`, `expect`, `vi`).
- Must use `@vue/test-utils` for mounting Vue components.

### File Naming and Location

- Base Directory: All test files MUST be placed in `src/__tests__/`.
- Mirrored Structure: The directory structure within `src/__tests__/` MUST mirror the directory structure of the file being tested from `src/`.
- File Suffix: All test files MUST end with the `.spec.js` suffix.

#### Examples

- "src/components/shared/BaseButton.vue -> src/**tests**/components/shared/BaseButton.spec.js"
- "src/services/users.service.js -> src/**tests**/services/users.service.js"
- "src/App.vue -> src/**tests**/App.spec.js"

### Test Development

- Structure: Clearly structure tests using the Arrange, Act, Assert (AAA) pattern.
- Coverage: Ensure testing of correct rendering, user interactions (clicks, inputs), props, events, and edge cases/error conditions.
- Mocks: Use `vi.mock()` for external dependencies, services, or modules.

### Run tests

- Use command `.\node_modules\.bin\vitest.cmd run`
