---
name: trip-budget-analyst
description: "여행 계획 하네스 Phase 4 Budget Analyst에서 사용한다. 예산 분석, 예산 계산, 비용 산정, 숙박비 계산 요청에 반응하며 공통 budget contract/procedure에 따라 artifacts/{run-id}/04-budget.md를 작성한다."
---

# Trip Budget Analyst Skill

Phase 4 Budget Analyst의 Codex 어댑터다.

## 필수로 읽을 파일

아래 파일을 순서대로 읽는다.

1. `harness/contracts/budget-analyst.contract.md`
2. `harness/procedures/budget-analyst.md`
3. `harness/schemas/budget.schema.md`
4. `artifacts/{run-id}/01-intake.md`
5. `artifacts/{run-id}/03-itinerary.md`

## 실행 절차

1. intake와 itinerary 산출물이 있는지 확인한다.
2. 공통 budget analyst contract와 procedure를 그대로 따른다.
3. itinerary에서 비용 항목을 추출하고 카테고리별 합계를 계산한다.
4. 저장 전 산술 계산을 자체 점검한다.
5. 예산 분석을 `artifacts/{run-id}/04-budget.md`에 저장한다.

## Codex 어댑터 규칙

- `harness/contracts/budget-analyst.contract.md`와 `harness/procedures/budget-analyst.md`를 단일 원본으로 따른다.
- 이 Phase는 공통 contract가 요구하는 budget 산출물 작성으로 제한한다.
- 이 Phase Skill에서 `harness/`나 `.claude/`를 수정하지 않는다.

## 출력

작성 파일:

- `artifacts/{run-id}/04-budget.md`

Report Phase로 넘길 짧은 요약을 반환한다.
