---
name: development-flow
description: Allreva BE에서 여러 파일·모듈·운영 영향이 있는 변경을 계획, 구현, 검증할 때 사용한다. Allreva_Harness의 공통 실행 흐름과 현재 프로젝트 규칙을 함께 적용한다.
---

# Allreva BE Development Flow

이 Skill은 공통 흐름을 위한 연결층이다. 한 파일의 명확한 수정처럼 작은 작업에는 사용하지 않는다.

1. 현재 프로젝트의 `AGENTS.md`를 읽고 Docs와 Harness 경로를 계산한다.
2. `$HARNESS_ROOT/skills/development-flow/SKILL.md`를 읽어 공통 흐름을 따른다.
3. 구현과 검증은 현재 프로젝트의 테스트·Git 규칙을 우선한다.

Harness 경로가 없으면 추측하지 말고 사용자에게 위치를 확인한다.
