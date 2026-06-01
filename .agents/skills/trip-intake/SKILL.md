---
name: trip-intake
description: "여행 계획 하네스 Phase 1 Intake에서 사용한다. 여행 조건 수집, 입력 수집, 누락 정보 확인 요청에 반응하며 공통 intake contract/procedure에 따라 artifacts/{run-id}/01-intake.md를 작성한다."
---

# Trip Intake Skill

Phase 1 Intake의 Codex 어댑터다.

## 필수로 읽을 파일

아래 파일을 순서대로 읽는다.

1. `harness/contracts/intake.contract.md`
2. `harness/procedures/intake.md`
3. `harness/schemas/intake-questionnaire.schema.md`
4. `harness/schemas/intake.schema.md`

## 실행 절차

1. Orchestrator 맥락에서 `run-id`를 확인하거나 `harness/ORCHESTRATOR.md` 기준으로 만든다.
2. `artifacts/{run-id}/01-intake.md`가 이미 있는지 확인한다.
3. 공통 intake contract와 procedure를 그대로 따른다.
4. 누락된 항목만 간결하게 질문한다.
5. 완성된 intake 파일을 `artifacts/{run-id}/01-intake.md`에 저장한다.

## Codex 어댑터 규칙

- `harness/contracts/intake.contract.md`와 `harness/procedures/intake.md`를 단일 원본으로 따른다.
- 사용자에게 질문해야 할 때는 공통 intake 절차를 따른다.
- 이 Phase Skill에서 `harness/`나 `.claude/`를 수정하지 않는다.

## 출력

작성 파일:

- `artifacts/{run-id}/01-intake.md`

다음 Phase로 넘길 짧은 요약을 반환한다.
