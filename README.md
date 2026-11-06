# Job Portal Backend

A Java / Spring Boot backend for a job portal, developed while following an Eazy Bytes Spring Boot course. The project focuses on REST APIs, relational data modeling, validation, and Spring Security.

**Status:** actively in development. Company listings and contact submissions are implemented; authentication is being completed. Section 12 (database authentication and CSRF protection) is planned for completion during the week of November 2, 2026.

## Tech stack

- Java 21, Spring Boot 4.0.8, Spring MVC
- Spring Data JPA / Hibernate, MySQL
- Spring Security, BCrypt, JJWT
- Jakarta Bean Validation, Springdoc OpenAPI
- Maven Wrapper, Docker Compose

This repository contains the backend. The frontend is maintained separately.

## Current functionality

- **Company listings:** retrieve companies with their associated jobs, mapped from JPA entities into response DTOs.
- **Contact submissions:** validate submitted fields and persist contact requests in MySQL.
- **API structure:** controller–service–repository layers, centralized exception handling, and media-type API versioning.
- **Security groundwork:** JWT generation and a token-validation filter, BCrypt password encoding, and public/authenticated route configuration.

### Authentication status

The current main branch includes a login controller and JWT infrastructure, but its AuthenticationManager has no authentication provider configured yet. Login is therefore **not ready for end-to-end use**.

User/role entities and repositories are present. Database-backed login, registration, duplicate-user checks, compromised-password checks, and CSRF protection are the next course milestones. CSRF is currently disabled in the security configuration.

## Architecture


```text
HTTP request
    -> Spring Security filter chain
    -> Controller
    -> Service
    -> Spring Data JPA repository
    -> MySQL
```

DTOs separate API responses from persistence entities. The company response includes associated job data through the company–job relationship.

## Run locally

### Prerequisites

- JDK 21
- Docker with Docker Compose
- An available MySQL port (default: 3306)

### 1. Clone the repository


```bash
git clone https://github.com/WhereAmIHeaded/job-portal.git
cd job-portal
```

### 2. Configure the database and JWT secret

The checked-in compose.yaml uses an author-specific host path for MySQL storage. Change that bind-mount path to a local directory on your machine before starting the application.

Set MYSQL_ROOT_PASSWORD and DATABASE_PASSWORD to matching local database passwords, and set JWT_SECRET to a random secret at least 32 bytes long. Pass these variables to the terminal or IDE process launching Spring Boot. A root-level .env file used by Docker Compose does not automatically inject variables into a separately launched Java process.

| Variable | Purpose / default |
| --- | --- |
| MYSQL_ROOT_PASSWORD | Required by the MySQL Compose service |
| DATABASE_HOST | Database host; defaults to localhost |
| DATABASE_PORT | Database port; defaults to 3306 |
| DATABASE_NAME | Database name; defaults to jobportal |
| DATABASE_USERNAME | Database user; defaults to root |
| DATABASE_PASSWORD | Password used by the application; must match the database user |
| JWT_SECRET | Signing key used by the JWT utility |

### 3. Start the application

Run ./mvnw spring-boot:run from the repository root. On Windows, use mvnw.cmd spring-boot:run. Spring Boot's Docker Compose integration is configured to start and stop the database service with the application.

The default application address is http://localhost:8080. Database schema and sample-data scripts are provided under src/main/resources/sql/; they are not configured for automatic import. Review them before loading sample data into a local database.

## API examples

Endpoints are prefixed with /api. Version 1.0 is the default; it can also be requested explicitly with the Accept header: application/vnd.eazyapp+json;v=1.0.

| Method | Endpoint | Status |
| --- | --- | --- |
| GET | /api/companies/public | Company listings with associated jobs |
| POST | /api/contacts/public | Validated contact submission |
| POST | /api/auth/login/public | Login/JWT flow under development |

Contact request fields: email, message (5–500 characters), name (5–30), subject (5–150), and userType. Accepted userType values are Job Seeker, Employer, and Other.

## Next milestone: Section 12

Planned for the week of November 2, 2026:

- [ ] Complete user registration with database persistence.
- [ ] Prevent duplicate users with derived repository queries.
- [ ] Add compromised-password checks.
- [ ] Configure database authentication and an authentication provider.
- [ ] Complete the CSRF protection exercises and integration.

These items describe planned work, not completed features. Later job-posting, application, and profile workflows will be documented as they are implemented.

## Learning context

This is a course-based project used to practice Java backend development. The repository is evolving alongside the course; it is not presented as a production deployment. Course examples are adapted and explored as part of that learning process.
