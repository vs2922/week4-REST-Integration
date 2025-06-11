````markdown
# PharmaInventory: Product Catalog Service

A Spring Boot–based microservice powering the Product Catalog for the Pharmaceutical Distribution Center Inventory System.  
This service provides robust CRUD operations on Medicines, Pallets, and Tasks, complete with:

- **Validation**: JSR-380 annotations and uniform error handling  
- **Documentation**: Live Swagger UI and OpenAPI spec  
- **Security**: HTTP Basic login + stateless JWT authentication  
- **Advanced REST**: JSON-Patch partial updates, HATEOAS links, ETag & caching  
- **Integration**: End-to-end tests covering auth, CRUD, patch, pagination, and docs  

---

## Table of Contents

1. [Features](#features)  
2. [Getting Started](#getting-started)  
3. [Configuration](#configuration)  
4. [Running the Service](#running-the-service)  
5. [API Reference](#api-reference)  
6. [Integration Tests](#integration-tests)  
7. [Project Structure](#project-structure)  
8. [Contributing](#contributing)  
9. [License](#license)  

---

## Features

- **DTO Validation**:  
  - `@NotBlank`, `@NotNull`, `@NotEmpty`, container-element constraints  
  - Unified `ErrorResponse` JSON schema via `GlobalExceptionHandler`

- **Swagger / OpenAPI**:  
  - Automatic UI at `/swagger-ui/index.html`  
  - Model- and operation-level annotations (`@Operation`, `@Schema`)

- **Security**:  
  - `/auth/login` with HTTP Basic  
  - JWT issuance and validation via `JwtUtil` & `JwtAuthenticationFilter`  
  - Stateless sessions, protected `/api/**` endpoints

- **Advanced REST Patterns**:  
  - JSON-Patch support (`PATCH /api/medicines/{id}`)  
  - ETag & conditional GET with `ShallowEtagHeaderFilter`  
  - HATEOAS pagination (`/api/medicines?page=&size=`)

- **Integration Testing**:  
  - `TestRestTemplate` end-to-end tests for auth, CRUD, patch, docs  
  - Embedded Kafka (planned in Week 5) and CI-driven OpenAPI validation  

---

## Getting Started

### Prerequisites

- Java 17  
- Maven 3.8+  
- PostgreSQL 12+ (or Docker Compose)  

### Clone & Build

```bash
git clone https://github.com/<your-username>/pharmainventory-catalog.git
cd pharmainventory-catalog
mvn clean package
````

---

## Configuration

All settings live under `src/main/resources`:

* **`application.yml`** – common defaults
* **`application-dev.yml`** – local Postgres, `ddl-auto=update`, SQL logging, CORS
* **`application-prod.yml`** – external credentials, `ddl-auto=validate`, log level

Activate profiles with:

```bash
# Local development
-Dspring.profiles.active=dev

# Production
-Dspring.profiles.active=prod
```

---

## Running the Service

```bash
java -jar target/inventory-system-0.3.0.jar \
  --spring.profiles.active=dev
```

Access:

* Swagger UI: `http://localhost:8080/swagger-ui/index.html`
* API Docs (JSON): `http://localhost:8080/v3/api-docs`
* H2 Console (if enabled): `http://localhost:8080/h2-console`

---

## API Reference

### Authentication

| Method | Path          | Description                             |
| ------ | ------------- | --------------------------------------- |
| POST   | `/auth/login` | Obtain JWT with `username` + `password` |

### Medicines

| Method | Path                  | Description                    |
| ------ | --------------------- | ------------------------------ |
| GET    | `/api/medicines`      | Paginated list (HATEOAS links) |
| GET    | `/api/medicines/{id}` | Retrieve a single medicine     |
| POST   | `/api/medicines`      | Create new medicine            |
| PUT    | `/api/medicines/{id}` | Full update                    |
| PATCH  | `/api/medicines/{id}` | Partial update (JSON-Patch)    |
| DELETE | `/api/medicines/{id}` | Delete medicine                |

*Additional endpoints for Pallets and Tasks follow the same CRUD pattern under `/api/pallets` and `/api/tasks`.*

---

## Integration Tests

Run all integration and unit tests:

```bash
mvn test
```

Key integration scenarios:

* Full auth → create → fetch → delete flow
* Pagination + HATEOAS link validation
* ETag conditional GET + JSON-Patch update
* Swagger UI availability

---

## Project Structure

```
pharmainventory-catalog/
├── pom.xml
├── src
│   ├── main
│   │   ├── java/com/pharmainventory/inventory
│   │   │   ├── config           # CORS, ETag, OpenAPI, Security
│   │   │   ├── controller       # REST endpoints + auth
│   │   │   ├── dto              # Validation DTOs
│   │   │   ├── exception        # GlobalExceptionHandler
│   │   │   ├── mapper           # MapStruct mappers
│   │   │   ├── security         # JWT util + filter
│   │   │   ├── service          # Business logic + patch support
│   │   │   └── model            # JPA entities
│   │   └── resources
│   │       ├── application*.yml
│   └── test
│       └── java/com/pharmainventory/inventory
│           ├── integration     # TestRestTemplate tests
│           └── controller      # MockMVC tests
└── README.md
```

---

## Contributing

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/foo`)
3. Commit your changes (`git commit -am 'Add feature'`)
4. Push (`git push origin feature/foo`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License. See `LICENSE` for details.

---

*Happy coding!*
