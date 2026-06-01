---
name: trip-researcher
description: "여행 계획 하네스 Phase 2 Researcher에서 사용한다. 조사, 여행 정보 수집, 가격·운영시간·예약 조건 확인 요청에 반응하며 공통 researcher contract/procedure에 따라 artifacts/{run-id}/02-research.md를 작성한다."
---

# Trip Researcher Skill

Phase 2 Researcher의 Codex 어댑터다.

## 필수로 읽을 파일

아래 파일을 순서대로 읽는다.

1. `harness/contracts/researcher.contract.md`
2. `harness/procedures/researcher.md`
3. `harness/schemas/research.schema.md`
4. `artifacts/{run-id}/01-intake.md`

## 실행 절차

1. `artifacts/{run-id}/01-intake.md`가 있는지 확인한다.
2. 공통 researcher contract와 procedure를 그대로 따른다.
3. 웹 조사 도구가 사용 가능하면 가격, 운영 시간, 예약 조건 확인에 우선 사용한다.
4. 확인하지 못한 최신 정보는 `확인 필요`로 분리한다.
5. 조사 결과를 `artifacts/{run-id}/02-research.md`에 저장한다.

## Codex 어댑터 규칙

- `harness/contracts/researcher.contract.md`와 `harness/procedures/researcher.md`를 단일 원본으로 따른다.
- 이 Phase는 공통 contract가 요구하는 research 산출물 작성으로 제한한다.
- 이 Phase Skill에서 `harness/`나 `.claude/`를 수정하지 않는다.

## 출력

작성 파일:

- `artifacts/{run-id}/02-research.md`

Planner에게 넘길 짧은 요약을 반환한다.
