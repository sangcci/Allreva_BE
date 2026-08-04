# AGENTS.md

## Project Scope

This file is a navigation layer for Allreva BE. Human-readable rules and decisions live in `Allreva_Docs`; executable workflows and agent roles live in `Allreva_Harness`.

## Documentation and Harness Roots

Resolve the primary worktree first, then use the environment override when it is set:

```bash
MAIN_WORKTREE=$(git worktree list --porcelain | awk '/^worktree / {print $2; exit}')
DOCS_ROOT="${ALLREVA_DOCS_ROOT:-$(dirname "$MAIN_WORKTREE")/Allreva_Docs}"
HARNESS_ROOT="${ALLREVA_HARNESS_ROOT:-$(dirname "$MAIN_WORKTREE")/Allreva_Harness}"
```

Before work, read `$DOCS_ROOT/INDEX.md` and the applicable current document. Do not use `archive/` as a current implementation rule. If either root is unavailable, ask the user for its location rather than guessing.

## Rules and Skills

- Java code or review: read `$DOCS_ROOT/rules/backend-code-conventions.md` and the project-local `backend-java-conventions` Skill.
- Tests: read `$DOCS_ROOT/rules/backend-test-conventions.md` and the project-local `backend-testing-conventions` Skill.
- Architecture or module-boundary changes: read `$DOCS_ROOT/architecture/` and `$DOCS_ROOT/decisions/` as applicable.
- Issue and PR work: read `$DOCS_ROOT/rules/`.
- Changes spanning multiple files, modules, or operational concerns: read the project-local `development-flow` Skill, which delegates to `$HARNESS_ROOT/skills/development-flow/SKILL.md`.
- Substantial diffs, unfamiliar areas, and PR preparation: use the Pi `explain-diff` agent exposed by `$HARNESS_ROOT`.
