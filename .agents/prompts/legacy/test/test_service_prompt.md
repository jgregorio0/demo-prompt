# Spec

## Role

Act as a QA developer specializing in Spring Framework legacy Testing.

## Task

Implement the Controller test for the Controller test class provided.
Test the acceptance criteria file provided.
Do not write production code or documentation, just the testing code.

## Context

- The Controller test class
  `com.preving.accionformativa.services.PtFormacionApiService`.
- The build file `build.gradle`.
- The acceptance criteria definition file `doc/pt_formacion_api_acceptance_criteria.md`.

## Constraints

- **Framework & Libraries**: JUnit 4 (use `@Before`, `@Test`), Mockito (use `mock()`, `when()`, `verify()`), and Spring
  Test.
- **Mocks**: Mock dependencies only. Data carrier classes, Plain Old Java Objects (POJOs), and domain models must be
  instantiated using their constructors with synthetic test data. Avoid using mocks.
- **Naming Convention**: Use `Should/When` pattern for test methods (e.g.,
  `handleRequestInternal_shouldResponseWithCertificado_whenSuccess`).
- **Structure**: Follow the Given/When/Then structure inside the tests using comments.
- **Assertions Helper**: Create a private helper methods to follow DRY on assertions.

## Steps to follow

1. **Context Assimilation**
    - read the context to understand the edge cases.

2. **Test Implementation**
    - Write the testing code following the exact structure and naming guidelines provided.
    - Test class must be
      `accion-formativa-wm/test/com.preving.accionformativa.services.PtFormacionApiServiceTest`.

4. **Implementation review**:
    - Self-review the generated code.
    - Run test
      `./gradlew test --tests "com.preving.accionformativa.services.PtFormacionApiServiceTest"`
    - If errors fix the errors and start 7. **Implementation review** until there is no errors.

## Output checklist

- [ ] Java test file code.
- [ ] No explanations, no markdown around the code (or just standard java code block), no production code.
