# Role

Act as a Senior Business Analyst and QA Lead specializing.

# Task

Analyze the provided Class and context, then generate a comprehensive "Acceptance Criteria" section on
`doc/acceptance-criteria.md`

# Context

- Class to test `com.preving.accionformativa.web.formacionpresencial.GenerarCertificadoController`

# Constraints

1. **Scenario Identification**: Identify the happy path (success) and critical edge cases/error paths (e.g., missing
   authentication, service failures, invalid parameters).
2. **BDD Structure**: Write each scenario strictly using **GIVEN**, **AND**, **WHEN**, **THEN** syntax.

# Output Format

- Provide only the Markdown file `doc/acceptance-criteria.md` with section:
```markdown
## Acceptance Criteria

### Scenario 1: [Descriptive Title for Success Path]

- **GIVEN** [Initial state, cookies, requests, parameters...]
- **AND** [Mock behaviors and return values...]
- **WHEN** the method `[methodName]` is executed.
- **THEN** [Expected assertion 1...]
- **AND** [Expected assertion 2 / Mock verifications...]

```