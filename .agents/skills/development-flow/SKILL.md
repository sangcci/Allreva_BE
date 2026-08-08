---
name: development-flow
description: Allreva BE에서 여러 파일·모듈·운영 영향이 있는 변경을 계획, 구현, 검증할 때 사용한다. Allreva_Harness의 공통 실행 흐름과 현재 프로젝트 규칙을 함께 적용한다.
---

# Allreva BE Development Flow

이 Skill은 공통 흐름 연결층이다. `AGENTS.md`의 의무 개발 workflow를 따른다.

1. 현재 프로젝트의 `AGENTS.md`를 읽고 Docs와 Harness 경로를 계산한다.
2. 관련 문서를 읽어 조사·논의를 끝내고 계획 승인을 받은 뒤 모든 Task의 Issue를 생성한다. 새 workflow에서는 RFC를 만들거나 연결하지 않으며, 과거 RFC는 근거 자료로만 사용한다. 오래 유지할 중요한 결정만 ADR로 기록한다.
3. 사용자에게 아래 문구로 workflow 진입 승인을 받고, 명시적 승인 전 구현을 시작하지 않는다.

   ```text
   이제 이 작업에 대해 개발 workflow로 전환해서 진행할까요?
   ```

4. 승인 뒤 `.allreva/git-workflow.json` 기준 branch를 검증하고, repository-local `.worktrees/`만 사용한다.
5. 구현·검증 뒤 PR 전 `explain-diff` 사용자 이해 승인을 받는다. 이어 제안 PR 제목·본문, 검증 근거, 잔여 위험을 제시한 뒤 별도 명시적 PR 생성 승인을 받는다. squash merge와 local cleanup은 각각 사용자 행동·cleanup signal 뒤에만 진행한다.
6. `$HARNESS_ROOT/skills/development-flow/SKILL.md`를 읽어 공통 역할 계약을 따른다. Project-tracked adapter는 `explain-diff`와 `git-workflow`를 제공한다. Pi는 scoped custom tool과 native confirmation을 사용하며, Codex·Claude는 host/user configuration으로 경계가 약화될 수 있는 policy-only adapter다.

구현과 검증은 현재 프로젝트 테스트·Git 규칙을 우선한다. Harness 경로가 없으면 추측하지 말고 사용자에게 위치를 확인한다.
