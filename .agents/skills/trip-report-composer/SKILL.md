---
name: trip-report-composer
description: "여행 계획 하네스 Phase 5 Report Composer에서 사용한다. HTML 리포트, 최종 리포트, 여행 계획 보고서 생성 요청에 반응하며 공통 report contract/procedure에 따라 artifacts/{run-id}/05-report.html을 작성한다."
---

# Trip Report Composer Skill

Phase 5 Report Composer의 Codex 어댑터다.

## 필수로 읽을 파일

아래 파일을 순서대로 읽는다.

1. `harness/contracts/report-composer.contract.md`
2. `harness/procedures/report-composer.md`
3. `harness/schemas/report.schema.md`
4. `artifacts/{run-id}/01-intake.md`
5. `artifacts/{run-id}/03-itinerary.md`
6. `artifacts/{run-id}/04-budget.md`

## 실행 절차

1. intake, itinerary, budget 산출물이 있는지 확인한다.
2. 공통 report composer contract와 procedure를 그대로 따른다.
3. 브라우저에서 열 수 있는 단일 HTML 리포트를 만든다.
4. 저장 전 필수 섹션과 모바일 가독성을 자체 점검한다.
5. 리포트를 `artifacts/{run-id}/05-report.html`에 저장한다.

## Codex 어댑터 규칙

- `harness/contracts/report-composer.contract.md`와 `harness/procedures/report-composer.md`를 단일 원본으로 따른다.
- 이 Phase는 공통 contract가 요구하는 report 산출물 작성으로 제한한다.
- 이 Phase Skill에서 `harness/`나 `.claude/`를 수정하지 않는다.

## 출력

작성 파일:

- `artifacts/{run-id}/05-report.html`

리포트 경로와 짧은 요약을 반환한다.
