---
name: trip-planner
description: "여행 계획 하네스 Phase 3 Planner에서 사용한다. 일정 작성, 일정표, 동선 구성, 여행 계획 초안 요청에 반응하며 공통 planner contract/procedure에 따라 artifacts/{run-id}/03-itinerary.md를 작성한다."
---

# Trip Planner Skill

Phase 3 Planner의 Codex 어댑터다.

## 필수로 읽을 파일

아래 파일을 순서대로 읽는다.

1. `harness/contracts/planner.contract.md`
2. `harness/procedures/planner.md`
3. `harness/schemas/itinerary.schema.md`
4. `artifacts/{run-id}/01-intake.md`
5. `artifacts/{run-id}/02-research.md`

## 실행 절차

1. intake와 research 산출물이 있는지 확인한다.
2. 공통 planner contract와 procedure를 그대로 따른다.
3. research 산출물을 후보 정보 원본으로 삼아 날짜별 일정을 구성한다.
4. 저장 전 이동 시간과 대안 일정 조건을 자체 점검한다.
5. 일정을 `artifacts/{run-id}/03-itinerary.md`에 저장한다.

## Codex 어댑터 규칙

- `harness/contracts/planner.contract.md`와 `harness/procedures/planner.md`를 단일 원본으로 따른다.
- 이 Phase는 공통 contract가 요구하는 itinerary 산출물 작성으로 제한한다.
- 이 Phase Skill에서 `harness/`나 `.claude/`를 수정하지 않는다.

## 출력

작성 파일:

- `artifacts/{run-id}/03-itinerary.md`

Budget Phase로 넘길 짧은 요약을 반환한다.
