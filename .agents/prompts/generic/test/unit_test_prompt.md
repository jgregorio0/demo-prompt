# Role

Act as a QA developer specializing in Spring Framework Testing.

# Task

Implement the unit test for the class provided.
Do not write production code or documentation, just the testing code.

# Context

- The class 
- The method createSessionActivity of controller [SessionActivityController.java](../../../src/main/java/com/vitaly/gef/infrastructure/api/controller/calendar/session/SessionActivityController.java)
- The Test class [CreateSessionActivityIT.java](../../../src/integrationTest/java/com/vitaly/gef/infrastructure/api/controller/calendar/session/CreateSessionActivityIT.java)
- The build file [build.gradle](../../../build.gradle)
- The acceptance criteria definition file [create_activity_acceptance_criteria.md](../../../docs/create_activity_acceptance_criteria.md)
- The AGENTS.md file [AGENTS.md](../../../AGENTS.md)
- H2 database definition [schema_h2.sql](../../../src/integrationTest/resources/db/schema_h2.sql)
- To preauthorize request some TechnicalPrevention.ANY role extends [1
- .java](../../../src/integrationTest/java/com/vitaly/gef/common/support/auth/PreventionTechnicianAuthorizedBaseIT.java)

# Constraints

- Add test cases on the test  class CreateSessionActivityIT provided only.
- Implement the scenarios described. Do not add new scenarios.

# Test format

```java
@SqlMergeMode(SqlMergeMode.MergeMode.MERGE)
@Sql(
    scripts = {
      "/db/cleanup.sql",
      // add initial scripts
    },
    executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD)
public class TestIT extends PreventionTechnicianAuthorizedBaseIT {

  private static final String ENDPOINT = "/sessions/activities";

  @Autowired private SessionActivityJpaRepository sessionActivityJpaRepository;

  @Test
  @Sql(scripts = {"/test-data/session/create_session_activity.sql"})
  void createSessionActivity_shouldPersistActivity_whenTimeRecordsEmpty() {
    // Given
    CreateSessionActivityRequest request =
        new CreateSessionActivityRequest(
            1L,
            10L,
            LocalDateTime.of(2026, 5, 19, 9, 0),
            LocalDateTime.of(2026, 5, 19, 10, 0),
            "Observations Test 1",
            true,
            List.of());

    // When
    ResponseEntity<SessionActivity> response =
        getRestTemplate().postForEntity(ENDPOINT, request, SessionActivity.class);

    // Then
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    assertThat(response.getBody()).isNotNull();
    assertThat(response.getBody().sessionId()).isEqualTo(1L);
    assertThat(response.getBody().activity().id()).isEqualTo(10L);
    assertThat(response.getBody().observations()).isEqualTo("Observations Test 1");
    assertThat(response.getBody().timeRecords()).isEmpty();

    // Verify DB
    List<SessionActivityEntity> dbActivities = sessionActivityJpaRepository.findBySessionId(1L);
    assertThat(dbActivities).hasSize(1);
    SessionActivityEntity savedEntity = dbActivities.get(0);
    assertThat(savedEntity.getActivity().getId()).isEqualTo(10L);
    assertThat(savedEntity.getObservations()).isEqualTo("Observations Test 1");
    assertThat(savedEntity.getTimeRecords()).isEmpty();
  }

  @Test
  @Sql(scripts = {"/test-data/session/create_session_activity.sql"})
  void createSessionActivity_shouldPersistActivityAndTimeRecords_whenTimeRecordsNotEmpty() {
    // Given
    CreateSessionTimeRecordRequest timeRecordRequest =
        new CreateSessionTimeRecordRequest(
            LocalDateTime.of(2026, 5, 19, 10, 0), BigDecimal.valueOf(2.5), "Time Record 1");

    CreateSessionActivityRequest request =
        new CreateSessionActivityRequest(
            1L,
            10L,
            LocalDateTime.of(2026, 5, 19, 9, 0),
            LocalDateTime.of(2026, 5, 19, 10, 0),
            "Observations Test 2",
            true,
            List.of(timeRecordRequest));

    // When
    ResponseEntity<SessionActivity> response =
        getRestTemplate().postForEntity(ENDPOINT, request, SessionActivity.class);

    // Then
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    assertThat(response.getBody()).isNotNull();
    assertThat(response.getBody().sessionId()).isEqualTo(1L);
    assertThat(response.getBody().activity().id()).isEqualTo(10L);
    assertThat(response.getBody().observations()).isEqualTo("Observations Test 2");
    assertThat(response.getBody().timeRecords()).hasSize(1);
    assertThat(response.getBody().timeRecords().get(0).hours()).isEqualByComparingTo("2.5");
    assertThat(response.getBody().timeRecords().get(0).description()).isEqualTo("Time Record 1");

    // Verify DB
    List<SessionActivityEntity> dbActivities = sessionActivityJpaRepository.findBySessionId(1L);
    assertThat(dbActivities).hasSize(1);
    SessionActivityEntity savedEntity = dbActivities.get(0);
    assertThat(savedEntity.getActivity().getId()).isEqualTo(10L);
    assertThat(savedEntity.getObservations()).isEqualTo("Observations Test 2");
    assertThat(savedEntity.getTimeRecords()).hasSize(1);
    SessionTimeRecordEntity savedTimeRecord = savedEntity.getTimeRecords().get(0);
    assertThat(savedTimeRecord.getHours()).isEqualByComparingTo("2.5");
    assertThat(savedTimeRecord.getDescription()).isEqualTo("Time Record 1");
  }

  @Test
  @Sql(scripts = {"/test-data/session/create_session_activity.sql"})
  void createSessionActivity_shouldRollback_whenDatabaseExceptionOccurs() {
    // Given
    // Using a non-existent activityId to trigger a foreign key constraint violation
    long nonExistentActivityId = 999999L;
    CreateSessionActivityRequest request =
        new CreateSessionActivityRequest(
            1L,
            nonExistentActivityId,
            LocalDateTime.of(2026, 5, 19, 9, 0),
            LocalDateTime.of(2026, 5, 19, 10, 0),
            "Observations Rollback Test",
            true,
            List.of());

    // When
    ResponseEntity<String> response =
        getRestTemplate().postForEntity(ENDPOINT, request, String.class);

    // Then
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.INTERNAL_SERVER_ERROR);

    // Verify DB: Ensure no activity was persisted for this session
    List<SessionActivityEntity> dbActivities = sessionActivityJpaRepository.findBySessionId(1L);
    assertThat(dbActivities).isEmpty();
  }

}

```

# Steps to follow

1. **Context Assimilation**
    - read the context to understand the happy path and the edge cases.

2. **SQL Queries**
    - Provide the given database content
    - Create a sql file for each insert.
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
