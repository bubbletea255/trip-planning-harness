---
name: trip-planning-orchestrator
description: "여행 계획 요청, /trip-planning, 재실행, 부분 수정, Claude/Codex 비교 요청에서 사용한다. harness/ORCHESTRATOR.md를 읽고 공통 업무 규칙을 복사하지 않은 채 Phase별 Codex Skill로 라우팅한다."
---

# Trip Planning Orchestrator Skill

여행 계획 하네스의 Codex 진입점이다.

## 필수로 읽을 파일

실행 또는 수정 전에 아래 파일을 읽는다.

1. `harness/ORCHESTRATOR.md` - Phase 순서, 실행 모드, 의존 관계, 재시도 규칙
2. `harness/MANIFEST.md` - Phase별 파일 지도
3. `artifacts/README.md` - 기존 실행 폴더와 산출물 규칙
4. `artifacts/improvement-log.md` - 이전 문제와 개선 기록

## 실행 절차

1. 요청을 새 실행, 재실행, 부분 수정, 리포트만 재생성, 비교 모드 중 하나로 분류한다.
2. Phase 0부터 Phase 7까지 `harness/ORCHESTRATOR.md`를 따른다.
3. 각 Phase에서 필요하면 아래 Codex Skill을 사용한다.
   - `trip-intake`
   - `trip-researcher`
   - `trip-planner`
   - `trip-budget-analyst`
   - `trip-report-composer`
   - `trip-judge`
4. 산출물은 기존 실행을 덮어쓰지 않고 `artifacts/{run-id}/` 아래에 저장한다. 재실행은 `-v2`, `-v3` 또는 플랫폼 접미사를 사용한다.
5. 의미 있는 새 실행 결과나 개선점이 생기면 `artifacts/README.md`와 `artifacts/improvement-log.md`를 갱신한다.

## Codex 어댑터 규칙

- 여기서 공통 하네스를 새로 설계하지 않는다.
- `harness/contracts/`, `harness/procedures/`, `harness/schemas/`, `harness/rubrics/`의 업무 규칙을 이 Skill에 길게 복사하지 않는다.
- 사용자가 명시적으로 요청하지 않는 한 `.claude/agents/`를 수정하지 않는다.
- 공통 규칙이 이상해 보이면 조용히 바꾸지 말고 `harness/` 변경 제안으로 보고한다.
- 외부 게시, 삭제, git 작업, Claude/Codex 최종안 선택 전에는 사용자 확인을 받는다.

## 출력

실행 후 아래 경로를 짧게 요약해 안내한다.

- `artifacts/{run-id}/05-report.html`
- `artifacts/{run-id}/03-itinerary.md`
- `artifacts/{run-id}/04-budget.md`
- 비교 모드에서는 `artifacts/comparison-{run-id}.md`
