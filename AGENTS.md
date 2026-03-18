# AGENTS.md (v3.1 – Practical & Enforceable)

## 1. Project Overview

This repository contains **WAPs**, a backend web service for:

* Project posting and management
* Team building
* Voting for outstanding projects

Codex operates as an engineering assistant and must follow **clear, enforceable, and practical rules**.
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
* `service` → business logic
* `repository` → persistence (JPA)
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

Before completing any task:

* [ ] Project builds successfully
* [ ] All tests pass
* [ ] No runtime errors introduced

---

## 5. Controller Rules

Controllers MUST:

* Handle request/response mapping
* Perform input validation
* Call service layer

Controllers MUST NOT:

* Contain core business logic
* Access repository directly

Controllers MAY:

* Use simple conditionals for validation or response mapping
* Transform DTOs

---

## 6. Service Rules

Service layer MUST:

* Contain all business logic
* Coordinate between repositories
* Use `@Transactional` for write operations when necessary

---

## 7. Database Rules

* Schema changes are NOT allowed without explicit instruction
* Maintain existing constraints and indexes
* Consider transaction boundaries when modifying data

---

## 8. Query & Performance Guidelines

* List queries SHOULD use pagination where applicable
* Avoid obvious N+1 query patterns

If N+1 is unavoidable:

* Add a comment explaining the reason
* Ensure the impact is limited (non-critical path or small dataset)

---

## 9. Error Handling (Current State)

This project does NOT yet have a global error handling system.

Codex must:

* Keep error handling consistent within the same endpoint
* Avoid mixing response formats (DTO vs raw string) within one API
* Avoid introducing large error-handling frameworks unless requested

---

## 10. Logging Rules

Log when necessary:

* Key business actions (e.g., create/update operations)
* Failures or unexpected states

Do NOT log:

* passwords
* tokens
* sensitive user data

---

## 11. Code Style Guidelines (STRICT)

### Control Flow

* Prefer `if-else` over ternary operators
* Use early return patterns to improve readability

Example:

```
if (invalid) {
    return;
}
```

---

### DTO Rules

* All DTOs MUST be implemented using `record`

Example:

```
public record UserResponse(Long id, String name) {}
```

---

### General Style

* Keep code readable and maintainable
* Prefer clear naming over abbreviations
* Avoid unnecessary abstraction

When introducing abstraction:

* Ensure it improves readability or reuse
* Avoid single-use helper methods unless justified

---

## 12. Testing Rules

### General

* New or modified logic SHOULD include tests
* Prefer integration tests for API behavior
* Unit tests SHOULD cover core service logic

---

### Required by Change Type

| Change Type   | Recommended Tests |
| ------------- | ----------------- |
| Service logic | Unit test         |
| API change    | Integration test  |
| Bug fix       | Reproduction test |

---

### Assertions

Prefer full object comparison when possible:

```
assertEquals(expected, actual);
```

---

## 13. API Compatibility Rules

* Existing response formats MUST NOT be broken
* New fields SHOULD be added in a backward-compatible way
* Do NOT remove or rename existing fields

---

## 14. Definition of Done

A task is complete if:

* [ ] Requirements are implemented
* [ ] Existing functionality is not broken
* [ ] All tests pass
* [ ] Code follows project conventions

---

## 15. Change Size Guidelines

Prefer small, incremental changes.

If a change is large:

* Break it into multiple steps when possible
* Ensure each step remains functional

---

## 16. Branch & Workflow Rules (MANDATORY)

Before starting any implementation:

* Codex MUST ask the user for approval before creating a new branch
* Codex MUST NOT start coding until approval is explicitly given

---

## 17. PR & Commit Message Rules (MANDATORY)

### Language

* All commit messages MUST be written in Korean
* All pull request titles and descriptions MUST be written in Korean

---

### Commit Message Format

```
<타입>: <변경 요약>

예:
feat: 투표 API 중복 검증 로직 추가
fix: 프로젝트 생성 시 null 예외 수정
```

---

### PR Description MUST include

* 변경 사항 (Summary)
* 테스트 내용 (Tests)
* 영향 범위 (Impact)
* 추가 제안 (Additional Suggestions, if applicable)

---

## 18. Exception Process

If a rule must be violated:

* Add a comment:

```
AGENTS_EXCEPTION: <reason>
```

* Keep the scope minimal
* Explain why the exception is necessary

---

## 19. Working Process (MANDATORY)

Codex MUST follow:

1. Identify relevant files
2. Understand current behavior
3. Ask for branch creation approval
4. Plan minimal changes
5. Implement changes
6. Add or update tests
7. Run tests
8. Verify no regressions

---

## 20. Continuous Improvement Note (IMPORTANT)

While working on the codebase, Codex SHOULD:

* Identify structural limitations
* Detect potential design issues
* Suggest better patterns or architectures

If such observations exist, Codex MUST:

* Add them at the end of the response as a separate section
* Clearly distinguish them from the main task result

Example:

```
[Additional Suggestions]
- Current voting logic may suffer from race conditions due to lack of DB constraints.
- Consider introducing a unique constraint or transactional redesign.
```

---

## 21. Summary

This document defines **practical and enforceable engineering rules**.

Rules are intended to guide consistent development without blocking normal work.
When necessary, use the exception process responsibly.

---
