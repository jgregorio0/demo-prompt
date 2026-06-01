# Role

Act as a QA developer specializing in Spring Framework legacy Testing.

# Task

Implement the integration test for the endpoint provided.
Use the acceptance criteria file provided.
Do not write production code or documentation, just the testing code.

# Context

- The endpoint POST /signatures
- The controller
  method [SignatureController.java](../../src/main/java/com/vitaly/pt/formacion/infrastructure/api/signature/SignatureController.java)
- The Test
  class [CreateSignatureContainerTest.java](../../src/integrationTest/java/com/vitaly/pt/formacion/signature/CreateSignatureContainerTest.java)
- The build file [build.gradle](../../build.gradle)
- The acceptance criteria definition
  file [create_signature_acceptance_criteria.md](../../docs/create_signature_acceptance_criteria.md).
- The AGENTS.md file [AGENTS.md](../../AGENTS.md).
- It is an integration Test using testcontainers for Oracle database and OpenOffice pdf generation service. Containers
  are defined
  in
  class [ContainerIT.java](../../src/integrationTest/java/com/vitaly/pt/formacion/common/support/container/ContainerIT.java)
- Oracle database definition [schema_oracle.sql](../../src/test/resources/schema_oracle.sql)

# Constraints

- Add test cases on the test
  class [CreateSignatureContainerIT.java](../../src/integrationTest/java/com/vitaly/pt/formacion/signature/CreateSignatureContainerIT.java)
  provided only.
- Implement the scenarios described. Do not add new scenarios.

# Steps to follow

1. **Context Assimilation**
    - read the context to understand the happy path and the edge cases.

2. **SQL Queries**
    - Provide the given database content
    - Create a query file for each insert.
    - Add it to each test using @Sql annotation.

2. **Test Implementation**
    - Write the testing code following the exact structure and naming guidelines provided.

3. **Implementation review**:
    - Self-review the generated code.
    - Run test
      `./gradlew integrationTest --tests "com.vitaly.pt.formacion.signature.CreateSignatureContainerIT"`

# Output checklist

- [ ] Java test file code.
- [ ] No explanations, no markdown around the code (or just standard java code block), no production code.
