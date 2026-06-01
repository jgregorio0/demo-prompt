# Role

Act as a QA developer specializing in Spring Framework legacy Testing.

# Task

Implement the integration test for the endpoint provided.
Use the acceptance criteria file provided.
Do not write production code or documentation, just the testing code.

# Context

- The endpoint POST /signatures
- The controller method `com.vitaly.pt.formacion.infrastructure.api.signature.SignatureController.create`.
- The Test class `com.vitaly.pt.formacion.signature.CreateSignatureContainerIT`
- The build file `build.gradle`.
- The acceptance criteria definition file `doc/create_signature_acceptance_criteria.md`.
- The AGENTS.md file `AGENTS.md`.
- It is an integration Test using testconteainers for Oracle database and OpenOffice pdf generation service. Containers
  are defined
  in class `com.vitaly.pt.formacion.common.support.container.ContainerIT`

# Constraints

- Add test cases on the test class `com.vitaly.pt.formacion.signature.CreateSignatureContainerIT` provided only.
- Implement the scenarios described. Do not add new scenarios.

# Steps to follow

1. **Context Assimilation**
    - read the context to understand the happy path and the edge cases.

2. **Test Implementation**
    - Write the testing code following the exact structure and naming guidelines provided.
    - Tests cases must be implemented in the provided Test class
      `com.vitaly.pt.formacion.signature.CreateSignatureContainerIT`.

3. **Implementation review**:
    - Self-review the generated code.
    - Run test
      `./gradlew integrationTest --tests "com.vitaly.pt.formacion.signature.CreateSignatureContainerIT"`

# Output checklist

- [ ] Java test file code.
- [ ] No explanations, no markdown around the code (or just standard java code block), no production code.
