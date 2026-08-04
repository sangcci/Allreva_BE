---
name: backend-java-conventions
description: Apply Allreva BE's Java 17, Spring, domain, transaction, exception, and Spotless conventions when writing or reviewing backend Java code.
---

# Allreva BE Java Conventions

Before writing or reviewing Java code, resolve `DOCS_ROOT` from `ALLREVA_DOCS_ROOT` or the parent of the primary worktree. Read `$DOCS_ROOT/rules/backend-code-conventions.md` as the human-readable source of truth.

Apply the current codebase pattern and that document before generic Java preferences. In particular:

- preserve the documented module dependency direction and domain/application/API/support responsibilities
- use `CustomException` with the domain `*ErrorCode` for business failures
- keep query transactions read-only and declare write transactions explicitly
- use records for DTOs unless a concrete framework or mutability constraint requires a class
- let Spotless and Palantir Java Format determine imports and whitespace

After Java changes, run the relevant Gradle tests and:

```bash
./gradlew spotlessApply
./gradlew spotlessCheck
```
