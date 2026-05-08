# test-agent

A Java Spring Boot microservice for user authentication using Oracle database.

## Features

- User registration and login
- Password hashing with BCrypt
- Oracle database integration
- RESTful API endpoints

## Prerequisites

- Java 17
- Maven
- Oracle Database
- Docker (for containerization)
- Jenkins (for CI/CD)

## Setup

1. Clone the repository
2. Configure Oracle database in `src/main/resources/application.properties`
3. Download Oracle JDBC driver and install to local Maven repository:
   ```
   mvn install:install-file -Dfile=ojdbc11.jar -DgroupId=com.oracle -DartifactId=ojdbc11 -Dversion=23.3.0.23.09 -Dpackaging=jar
   ```
4. Run the application:
   ```
   mvn spring-boot:run
   ```

## API Endpoints

- POST /auth/login?username=...&password=...
- POST /auth/register?username=...&password=...

## CI/CD

Jenkins pipeline is configured in `Jenkinsfile` for build, test, package, and deploy.

## Docker

Build and run with Docker:
```
docker build -t test-agent .
docker run -p 8080:8080 test-agent
```