# AGENTS.md - AI Coding Agent Guidelines for test-agent

## Architecture Overview
This is a Spring Boot 3.2.0 microservice for user authentication using Oracle database. Follows layered architecture: `AuthController` → `UserService` → `UserRepository` → `User` entity.

- **Data Flow**: REST endpoints in `AuthController.java` call `UserService.authenticate()` or `register()`, which interacts with JPA repository for Oracle DB.
- **Entity Design**: `User.java` uses username as primary key (String), stores BCrypt-hashed passwords.
- **API Pattern**: Endpoints use `@RequestParam` for username/password (e.g., `POST /auth/login?username=...&password=...`), not JSON bodies.

## Dependencies & Setup
- **Oracle JDBC**: Manually install `ojdbc11.jar` via `mvn install:install-file -Dfile=ojdbc11.jar -DgroupId=com.oracle -DartifactId=ojdbc11 -Dversion=23.3.0.23.09 -Dpackaging=jar` before building.
- **Database Config**: Update `src/main/resources/application.properties` with Oracle connection details; uses `ddl-auto=update` for schema management.
- **Security**: Relies on Spring Security defaults (admin/admin user); passwords hashed with `BCryptPasswordEncoder` in `UserService.java`.

## Build & Run Commands
- **Local Run**: `mvn spring-boot:run` (requires Oracle DB configured).
- **Package**: `mvn clean package -DskipTests` (skips tests for faster builds).
- **Docker Build/Run**: `docker build -t test-agent .` then `docker run -p 8080:8080 test-agent` (includes Maven build inside container).

## Testing & CI/CD
- **Test Execution**: `mvn test` runs JUnit 5 tests; only basic context load test exists in `TestAgentApplicationTests.java`.
- **CI Pipeline**: `Jenkinsfile` defines stages: compile, test, package, Docker build, deploy. Uses `junit` step for reports from `target/surefire-reports/*.xml`.
- **Debugging**: Enable SQL logging via `spring.jpa.show-sql=true` in `application.properties`.

## Code Patterns
- **Injection**: Use field injection with `@Autowired` (e.g., in `AuthController.java` and `UserService.java`).
- **Repository Methods**: Custom finder like `findByUsername` in `UserRepository.java` (extends `JpaRepository<User, String>`).
- **Response Handling**: Controllers return `ResponseEntity<String>` with status codes (e.g., 401 for invalid login).
- **Entity Conventions**: Simple POJOs with getters/setters; table name "users" via `@Table(name = "users")`.

## Key Files
- `pom.xml`: Spring Boot starters (web, data-jpa, security); Oracle JDBC dependency.
- `src/main/resources/application.properties`: DB config, JPA settings, server port 8080.
- `Jenkinsfile`: CI/CD pipeline with Maven and Docker steps.
- `Dockerfile`: Multi-stage build with Maven for dependency resolution.</content>
<parameter name="filePath">/Users/chennaposham/IdeaProjects/test-agent/AGENTS.md
