# AGENTS.md

## 1. Project Overview

This repository contains **WAPs**, a backend web service for:

* Project posting and management
* Team building
* Voting for outstanding projects

Codex operates as an engineering assistant and must follow **strict, enforceable rules**.
All changes must be **verifiable, testable, and non-breaking**.

---

## 2. Tech Stack

* Java 17
* Spring Boot
* Spring Data JPA
* MySQL
* Gradle

---

## 3. Repository Structure (Mandatory)

* `controller` → HTTP layer only
* `service` → business logic only
* `repository` → persistence only
* `dto` → request/response models

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

### Quality Gate (MANDATORY)

Before completing any task, ALL must pass:

* [ ] Project builds successfully
* [ ] All tests pass
* [ ] No compilation warnings

---

## 5. Controller Rules (Strict)

Controllers MUST:

* Only perform:

  * request validation
  * DTO mapping
  * service calls
  * HTTP response mapping

Controllers MUST NOT:

* Contain loops (`for`, `while`)
* Contain business condition branching (`if` related to domain logic)
* Access repository directly

Allowed example:

* null/format validation
* authentication extraction

Violation example:

* vote duplication logic in controller ❌

---

## 6. Service Rules (Strict)

Service layer MUST:

* Contain all business logic
* Be the only layer coordinating multiple repositories
* Use `@Transactional` for write operations

---

## 7. Voting System Constraints (CRITICAL – MUST ENFORCE)

### 7.1 Request Validation

* A vote request MUST contain exactly 3 project IDs
* All project IDs MUST be unique

Example (invalid):

```
[101, 101, 101] ❌
```

---

### 7.2 Database Constraints (MANDATORY)

The database MUST enforce uniqueness:

* `(user_id, poll_id, project_id)` must be UNIQUE

Without this constraint, the implementation is considered incorrect.

---

### 7.3 Concurrency Guarantee

Voting MUST be safe under concurrent requests.

At least ONE of the following MUST exist:

* DB unique constraint (required baseline)
* OR pessimistic/optimistic locking

---

### 7.4 Required Tests

The following tests are MANDATORY:

* Duplicate vote test (same user, same poll)
* Concurrent vote test (parallel requests)
* Validation test (duplicate project IDs rejected)

If these tests do not exist, the task is NOT complete.

---

## 8. Database Rules

* Schema changes are NOT allowed without explicit instruction
* All list queries MUST use:

  * pagination OR
  * fetch join

---

## 9. N+1 Query Prevention (Enforceable)

For any repository method returning collections:

* MUST use fetch join OR
* MUST use batch size OR
* MUST justify via comment

Verification method:

* Enable SQL logging
* No repeated SELECT per row allowed

---

## 10. Error Handling (Current Policy)

Since no global system exists:

* Controllers MUST return consistent response shape per endpoint
* Do NOT mix:

  * raw strings
  * DTO responses

Each endpoint must use ONE consistent response type.

---

## 11. Logging Rules

The following MUST be logged:

* vote submission
* vote failure
* critical business actions

The following MUST NOT be logged:

* passwords
* tokens
* sensitive user data

---

## 12. Code Style Rules (Enforceable)

### Method Size

* Maximum: 30 lines

### File Size

* Soft limit: 500 lines
* Hard limit: 800 lines → MUST split

### Complexity

* No nested `if` depth > 2

---

## 13. Testing Rules (Strict)

### Required by Change Type

| Change Type   | Required Tests                   |
| ------------- | -------------------------------- |
| Service logic | Unit test                        |
| API change    | Integration test                 |
| Voting logic  | Unit + Integration + Concurrency |

---

### Assertions

* Prefer full object comparison:

```
assertEquals(expected, actual);
```

---

## 14. API Compatibility Rules

* Existing response format MUST NOT change
* New fields MUST be additive only
* Do NOT remove or rename fields

---

## 15. Definition of Done (Strict)

A task is complete ONLY if:

* [ ] Requirements implemented
* [ ] All tests pass
* [ ] Required tests added
* [ ] No rule in this document is violated

---

## 16. Change Size Policy

A change is considered "small" ONLY if:

* ≤ 5 files modified
* ≤ 300 lines added/changed

If exceeded, the change MUST be split.

---

## 17. Exception Process

If a rule must be violated:

* The change MUST include a comment:

```
AGENTS_EXCEPTION: <reason>
```

Without this, the change is invalid.

---

## 18. Working Process (Mandatory)

Codex MUST follow:

1. Identify relevant files
2. Plan minimal change
3. Implement
4. Add required tests
5. Run tests
6. Verify all rules

---

## 19. Priority Rules

If conflicts occur:

1. Voting system constraints
2. Database constraints
3. API compatibility
4. Other rules

---

## 20. Summary

This document defines **strict, enforceable rules**.

Rules are NOT guidelines.
They are **requirements**.

Failure to comply means the task is incomplete.

---
