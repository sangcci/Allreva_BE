# AGENTS.md

## Project Scope

이 파일은 **Allreva_BE_Forked 프로젝트 전용 작업 규칙**을 정의한다.
전역 하네스 사용법이나 일반적인 워크플로우 강제보다,
이 프로젝트의 아키텍처·스타일·탐색 규칙을 우선한다.

## 문서 시스템

사람이 읽는 공식 기록은 `Allreva_Docs`에 둔다. worktree에서도 같은 위치를 찾기 위해 아래 순서로 문서 루트를 확인한다.

1. `ALLREVA_DOCS_ROOT` 환경 변수가 설정되어 있고 비어있지 않으면 그 경로를 사용한다.
2. 없거나 비어 있으면 primary worktree의 부모 디렉터리 아래 `Allreva_Docs`를 찾는다.

```bash
MAIN_WORKTREE=$(git worktree list --porcelain | awk '/^worktree / {print $2; exit}')
DOCS_ROOT="${ALLREVA_DOCS_ROOT:-$(dirname "$MAIN_WORKTREE")/Allreva_Docs}"
```

작업 전에는 `$DOCS_ROOT/INDEX.md`에서 현재 기준 문서를 찾는다. 해당 디렉터리가 없다면 추측하지 말고 사용자에게 위치를 확인한다.

- 아키텍처와 모듈 경계: `architecture/`
- Issue·PR·개발 규칙: `rules/`
- 큰 변경의 검토와 결정: `decisions/`
- `archive/`는 보존 자료다. 현재 구현 규칙이나 설계 근거로 사용하지 않는다.

중앙 Docs에 문서가 추가됐다고 이 파일을 항상 바꾸지 않는다. 문서 구조, 항상 지켜야 할 규칙, 반복 실행 절차처럼 Agent의 탐색이나 실행 방식이 바뀔 때만 갱신한다.

## Harness 실행 자산

Agent 실행 흐름은 로컬 `Allreva_Harness`에 둔다. 사람용 정책은 중앙 Docs를, 실행 절차는 Harness를 기준으로 한다.

1. `ALLREVA_HARNESS_ROOT` 환경 변수가 설정되어 있고 비어있지 않으면 그 경로를 사용한다.
2. 없거나 비어 있으면 primary worktree의 부모 디렉터리 아래 `Allreva_Harness`를 찾는다.

```bash
MAIN_WORKTREE=$(git worktree list --porcelain | awk '/^worktree / {print $2; exit}')
HARNESS_ROOT="${ALLREVA_HARNESS_ROOT:-$(dirname "$MAIN_WORKTREE")/Allreva_Harness}"
```

여러 파일·모듈·운영 영향이 있는 변경에서는 project-local `development-flow` Skill을 먼저 읽는다. 이 Skill은 `$HARNESS_ROOT/skills/development-flow/SKILL.md`를 읽는 얇은 연결층이다. 경로가 없으면 추측하지 말고 사용자에게 위치를 확인한다.

## 코드 스타일 (Spotless + Palantir)

프로젝트는 **Palantir Java Format**을 사용한다. 코드 작성 후 반드시 아래 명령을 실행한다.

```bash
./gradlew spotlessApply
./gradlew spotlessCheck
```

Palantir가 import 정렬, 미사용 import 제거, 공백/줄바꿈/들여쓰기를 자동 처리한다.
따라서 import 순서는 직접 맞추려 하지 말고 `spotlessApply` 결과를 따른다.

### Git Lint 도구

- **git-branch**: 이슈 번호와 컨벤션을 받아 최신 base 브랜치에서 네이밍 규칙에 맞는 브랜치 생성
- **git-commit**: git diff와 git log를 분석해 컨벤션에 맞는 커밋 메시지 생성, 여러 단위 변경 시 커밋 분리 제안
- **github-issue**: 이슈 번호(#12 형식)로 이슈 생성 후 branch 연결, 새 기능/버그 수정/개선 작업 시작 시 사용
- **github-pr**: 커밋과 diff 분석해 제목·본문·label 자동 구성, 프로젝트 AGENTS.md의 PR 컨벤션 우선 적용

## 아키텍처 제약

- **레이어**: `domain` → `application` → `presentation` / `infra`
- **역방향 의존 금지**
- **애그리거트 간 참조**: 직접 객체 참조 금지, ID로만 참조
- **예외 처리**: `CustomException` + 도메인별 `*ErrorCode` enum
- **Service**: `@Transactional(readOnly = true)` 기본, 쓰기 메서드만 `@Transactional`

## Git / Commit / PR

Git, commit, PR 규칙은 전역 컨벤션 또는 별도 합의된 규칙을 따른다.
이 파일에는 프로젝트 특화 규칙만 추가한다.

Issue와 PR은 정해진 항목을 기계적으로 길게 채우지 않는다. 변경 크기에 맞춰 실제로 바뀐 점, 확인한 내용, 남은 확인 사항을 짧고 사실적으로 쓴다.

### Branch / Worktree

- 기능 작업은 가능하면 현재 workspace에서 바로 브랜치를 따지 말고 **git worktree**로 분리한다.
- 기본 worktree 경로는 프로젝트 루트 기준 `.worktrees/{type}/{slug}` 형태를 사용한다.
  - 예: `.worktrees/feat/refresh-token-api-csrf`
- 브랜치명은 `{type}/#{issue-number}-{slug}` 패턴을 사용한다.
  - 예: `feat/#72-refresh-token-api-csrf`
- 메인 workspace는 가능하면 `develop` 상태로 유지한다.
