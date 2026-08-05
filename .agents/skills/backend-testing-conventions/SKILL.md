---
name: backend-testing-conventions
description: Apply Allreva BE's unit, DB slice, external adapter, and integration-test conventions when adding or reviewing Java tests.
---

# Allreva BE Testing Conventions

Before adding or reviewing a test, resolve `DOCS_ROOT` from `ALLREVA_DOCS_ROOT` or the parent of the primary worktree. Read `$DOCS_ROOT/rules/backend-test-conventions.md` as the human-readable source of truth.

Choose the narrowest test boundary that proves the changed behavior:

- unit test for pure domain behavior, mappers, and isolated collaborators
- `DataJpaTestSupport` for JPA repositories and DB adapters
- module test support with WireMock for external HTTP boundaries
- `IntegrationTestSupport` in `allreva-test` for application wiring across modules

Follow the documented JUnit 5, AssertJ, BDDMockito, Fixture, cleanup, and exception-assertion rules. Do not use a generic test directory template; place the test in the Gradle module that owns the production code.

Run the changed module's tests first, then the full required validation:

```bash
./gradlew :<module>:test
./gradlew test
./gradlew spotlessCheck
```
