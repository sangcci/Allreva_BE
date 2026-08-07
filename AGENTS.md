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

## Mandatory Development Workflow

Every task requires an Issue and RFC. Before implementation, complete research and a plan, then ask user exactly:

```text
이제 이 작업에 대해 개발 workflow로 전환해서 진행할까요?
```

Wait for explicit approval before workflow entry. After approval:

1. Validate branch against `.allreva/git-workflow.json`; create task worktrees only under ignored repository-local `.worktrees/`.
2. Implement and verify requested change.
3. Before PR creation, run `explain-diff` and obtain user understanding approval. Then present proposed PR title, body, validation evidence, and residual risks; obtain separate explicit PR-creation approval.
4. User squash-merges PR. Perform local worktree or branch cleanup only after user cleanup signal.

`$HARNESS_ROOT` supplies shared workflow guidance and role contracts. Project-tracked adapters expose `explain-diff` and `git-workflow`: Pi uses a scoped custom tool with native confirmation; Codex and Claude are policy-only and host/user configuration can weaken their boundary. `.pi/` local settings remain ignored. Existing GitHub title-lint workflows remain final remote title validation.

## Rules and Skills

- Java code or review: read `$DOCS_ROOT/rules/backend-code-conventions.md` and project-local `backend-java-conventions` Skill.
- Tests: read `$DOCS_ROOT/rules/backend-test-conventions.md` and project-local `backend-testing-conventions` Skill.
- Architecture or module-boundary changes: read `$DOCS_ROOT/architecture/` and `$DOCS_ROOT/decisions/` as applicable.
- Issue and PR work: read `$DOCS_ROOT/rules/`.
- Changes spanning multiple files, modules, or operational concerns: read project-local `development-flow` Skill, which delegates to `$HARNESS_ROOT/skills/development-flow/SKILL.md`.
- Substantial diffs, unfamiliar areas, and PR preparation: use `explain-diff` role exposed by `$HARNESS_ROOT` through configured runtime.
