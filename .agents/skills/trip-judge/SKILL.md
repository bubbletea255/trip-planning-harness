---
name: trip-judge
description: "여행 계획 하네스 Phase 6 Judge에서 사용한다. 비교, 교차 검증, Claude Codex 비교, 채점 요청에 반응하며 공통 judge contract/procedure/rubric에 따라 artifacts/comparison-{run-id}.md를 작성한다."
---

# Trip Judge Skill

Phase 6 Judge의 Codex 어댑터다.

## 필수로 읽을 파일

아래 파일을 순서대로 읽는다.

1. `harness/contracts/judge.contract.md`
2. `harness/procedures/judge.md`
3. `harness/rubrics/judge.rubric.md`
4. `harness/schemas/comparison.schema.md`
5. `artifacts/{run-id}-claude/`
6. `artifacts/{run-id}-codex/`

## 실행 절차

1. 두 플랫폼 결과 폴더가 모두 있는지 확인한다.
2. 공통 judge contract와 procedure를 그대로 따른다.
3. 채점에는 `harness/rubrics/judge.rubric.md`만 사용한다.
4. 각 점수마다 근거를 포함한다.
5. 비교 리포트를 `artifacts/comparison-{run-id}.md`에 저장한다.

## Codex 어댑터 규칙

- `harness/contracts/judge.contract.md`, `harness/procedures/judge.md`, `harness/rubrics/judge.rubric.md`를 단일 원본으로 따른다.
- 이 Phase는 공통 contract가 요구하는 comparison 산출물 작성으로 제한한다.
- 이 Phase Skill에서 `harness/`나 `.claude/`를 수정하지 않는다.

## 출력

작성 파일:

- `artifacts/comparison-{run-id}.md`

짧은 권고를 반환하되 최종안 선택 전에는 사용자 승인을 요청한다.
