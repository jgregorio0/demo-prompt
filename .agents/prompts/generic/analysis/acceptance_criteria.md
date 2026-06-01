# Role

Act as a Senior Business Analyst and QA Lead specializing.

# Task

Analyze the provided Class and context, then generate a comprehensive "Acceptance Criteria" documentation.

# Context

- Method to
  test [SignatureService.java](../../src/main/java/com/vitaly/pt/formacion/application/signature/SignatureService.java)
- Method onSignatureCreation in service
  [WorkerAttendanceService.java](../../src/main/java/com/vitaly/pt/formacion/application/attendance/worker/WorkerAttendanceService.java)
  listening to
  [SignatureCreationEvent.java](../../src/main/java/com/vitaly/pt/formacion/domain/signature/creation/SignatureCreationEvent.java)
- Method onWorkerAttendanceCreation in
  service [AttendanceService.java](../../src/main/java/com/vitaly/pt/formacion/application/attendance/AttendanceService.java)
  listening
  to [WorkerAttendanceCreationEvent.java](../../src/main/java/com/vitaly/pt/formacion/domain/attendance/worker/creation/WorkerAttendanceCreationEvent.java)
- Method onWorkerAttendanceCreation in
  service [CompletedSignatureAttendanceService.java](../../src/main/java/com/vitaly/pt/formacion/application/execution/enrollment/communication/CompletedSignatureAttendanceService.java)
  listening
  to [WorkerAttendanceCreationEvent.java](../../src/main/java/com/vitaly/pt/formacion/domain/attendance/worker/creation/WorkerAttendanceCreationEvent.java)
- The current acceptance
  criteria [create_signature_acceptance_criteria.md](../../docs/create_signature_acceptance_criteria.md)

# Constraints

1. **Scenario Identification**: Identify the happy path (success) and critical edge cases/error paths (e.g., missing
   authentication, service failures, invalid parameters).
2. **BDD Structure**: Write each scenario strictly using **GIVEN**, **AND**, **WHEN**, **THEN** syntax.

# Output Format

- the acceptance criteria resulting file must follow the format:

```markdown
## Acceptance Criteria

### Scenario 1: [Descriptive Title for Success Path]

- **GIVEN** [Initial state, cookies, requests, parameters...]
- **AND** [Mock behaviors and return values...]
- **WHEN** the method `[methodName]` is executed.
- **THEN** [Expected assertion 1...]
- **AND** [Expected assertion 2 / Mock verifications...]

```

# Output checklist

[] The file [create_signature_acceptance_criteria.md](../../docs/create_signature_acceptance_criteria.md) including up
to 6 acceptance criteria
   