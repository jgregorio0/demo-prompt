---
name: integration-testing
description: Generates Spring Boot integration test ensuring proper database state isolation and validation using SQL scripts and verifying using assertions.
  Use this skill when the user asks to generate integration tests.
compatibility: Java 18, Spring Boot 2.7.x, Gradle build system, H2 In-Memory Database.
---

# Spring Boot Integration Testing

## Purpose
This skill guides the agent in generating robust integration tests for Spring Framework REST endpoints, ensuring correct database interaction, authorization lifecycle, and transaction behavior (rollbacks) using dynamic SQL data injection.

## System Role Alignment
Act as a QA developer specializing in Spring Framework Integration Testing, maintaining database integrity, and utilizing standard AssertJ assertions.

---

## Input Prerequisites
To execute this skill successfully, the agent requires:
- Acceptance Criteria requirements.
- Target Controller and Method to test.
- Endpoint definition (Path and HTTP Method).
- Security base class to inherit (PreventionTechnicianAuthorizedBaseIT, CalendarManagerAuthorizedBaseIT)

---

## Implementation Steps

### 1. Context Assimilation
- Analyze the acceptance criteria, the target Controller, and DTO structures.
- Identify the happy paths, error boundaries, validation constraints, and database side-effects.

### 2. SQL Data Management
- Do not mix business domain states across test boundaries. Use explicit SQL script segregation.
- Create individual `.sql` files for specific test data setups inside `src/integrationTest/resources/test-data/...`.
- Leverage `@Sql` execution phases natively. Ensure a database cleanup script `src/integrationTest/resources/db/cleanup.sql` runs before each method execution.

### 3. Test Structure & Implementation
- Create a test class in `/src/integrationTest/java` within the same package as the Target class, with a name corresponding to the Target class and suffixed with `IT` (e.g., CreateSessionActivityIT).
- Inherit security base class (PreventionTechnicianAuthorizedBaseIT, CalendarManagerAuthorizedBaseIT, or the project-specific equivalent providing the pre-authenticated context).
- Inject required `JpaRepository` beans to directly verify the persistent state after API executions.
- Use the Given-When-Then BDD pattern inside every test block.
- **Assertions Requirement:** Validate the HTTP Response Status, the response Body payload properties, and the final state in the Database.

---

## Constraints

- **Strict Constraint:** Do not write production code or documentation. Generate **only** the integration test class implementation and its referenced SQL data setup snippets.
- Use `@SqlMergeMode(SqlMergeMode.MergeMode.MERGE)` to properly combine class-level cleanups with method-level setups.

### Structural Pattern

```java
@SqlMergeMode(SqlMergeMode.MergeMode.MERGE)
@Sql(
    scripts = {
      "/db/cleanup.sql"
    },
    executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD)
public class CreateSessionActivityIT extends PreventionTechnicianAuthorizedBaseIT {

  private static final String ENDPOINT = "/sessions/activities";

  @Autowired 
  private SessionActivityJpaRepository repository;

  @Test
  @Sql(scripts = {"/test-data/{{domain}}/{{setup_script}}.sql"})
  void create_shouldPersistActivity_whenTimeRecordsEmpty() {
    // Given
      CreateSessionActivityRequest request = new CreateSessionActivityRequest();

    // When
    ResponseEntity<CreateSessionActivityResponse> response =
        getRestTemplate().post(ENDPOINT, request, CreateSessionActivityResponse.class);

    // Then
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    assertThat(response.getBody()).isNotNull();
    // Add specific body assertions...

    // Verify DB State
    var dbEntities = repository.findById(response.getBody().id());
    assertThat(dbEntities).hasSize(1);
  }
}
```
