# AGENTS.md

## 1. Project Overview

This repository contains **WAPs**, a web service designed to support club operations, including:

* Project posting and management
* Team building
* Voting for outstanding projects

Codex operates as an engineering assistant in this repository.
Its responsibilities include:

* Understanding existing code
* Adding small features
* Fixing bugs
* Writing and improving tests

The primary goal is to maintain **correctness, consistency, and stability** of the service.

---

## 2. Tech Stack

* Java 17
* Spring Boot
* Spring Data JPA
* MySQL
* Gradle

---

## 3. Repository Structure

* `controller` → Handles HTTP requests/responses (must remain thin)
* `service` → Contains business logic
* `repository` → Handles database access (JPA)
* `dto` → Request/Response objects

---

## 4. Environment & Execution

### Run application

```
./gradlew bootRun
```

### Run tests

```
./gradlew test
```

Codex must always ensure that:

* The project builds successfully
* All tests pass after changes

---

## 5. Core Architectural Rules

### Layering

* Controllers must NOT contain business logic
* Business logic must exist in the service layer
* Repositories must only handle persistence

### DTO Usage

* Always use DTOs for API input/output
* Do not expose internal data structures directly

---

## 6. Voting System Constraints (Critical)

The voting system is a core feature of WAPs.

Codex MUST ensure:

* A user cannot vote multiple times for the same poll
* Vote aggregation must remain consistent
* Concurrency issues must be considered
* Database-level constraints (e.g., unique `(user_id, poll_id)`) must not be broken

---

## 7. Error Handling (Future Work)

This project does NOT yet have a standardized error handling system.

Codex must:

* Avoid introducing inconsistent error response formats
* Prefer simple and clear error handling
* If introducing error handling, keep it minimal and consistent across endpoints
* Do NOT design a full global exception system unless explicitly requested

---

## 8. Database Rules

* Do NOT modify schema without explicit instruction
* Maintain existing constraints and indexes
* Consider transaction boundaries (`@Transactional`)
* Prevent N+1 query problems

---

## 9. Logging

* Log important business events when necessary
* Do NOT log sensitive information (passwords, tokens, etc.)

---

## 10. Code Style & Design Rules

* Keep methods small and readable
* Avoid unnecessary abstraction
* Avoid ambiguous parameters such as:

  * `foo(false)`
  * `bar(null)`

Prefer:

* enums
* clearly named methods

---

## 11. File & Module Size Rules

* Keep files under ~500 lines when possible
* If a file exceeds ~800 lines, split it into smaller modules
* Prefer multiple focused classes over one large class

---

## 12. Testing Rules

* Every new feature must include tests
* Prefer integration tests for API behavior
* Prefer full object comparison over field-by-field assertions

Example:

```
assertEquals(expected, actual);
```

---

## 13. Constraints (Do NOT)

Codex must NEVER:

* Change API response formats without instruction
* Modify database schema arbitrarily
* Refactor unrelated code
* Introduce breaking changes
* Add large architectural changes without explicit request

---

## 14. Definition of Done

A task is complete only if:

* [ ] Requirements are fully satisfied
* [ ] Existing functionality is not broken
* [ ] All tests pass (`./gradlew test`)
* [ ] New tests are added when needed
* [ ] Code follows project conventions

---

## 15. Working Process for Codex

Codex must follow this workflow:

1. Read and understand related files
2. Identify affected areas
3. Create a minimal change plan
4. Implement changes with minimal scope
5. Add or update tests
6. Run tests
7. Fix any failures

---

## 16. Preferred Development Approach

* Prefer small, incremental changes
* Avoid large, sweeping modifications
* Prioritize readability and maintainability

---

## 17. Example Tasks

* Add project CRUD functionality
* Improve vote validation logic
* Prevent duplicate voting
* Refactor service layer logic
* Add missing test coverage
* Fix API edge-case bugs

---
