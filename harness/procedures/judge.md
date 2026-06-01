# Judge 수행 절차

1. `harness/rubrics/judge.rubric.md`를 읽어 5개 평가 항목과 가중치를 확인한다.
2. Claude 결과셋(`artifacts/{run-id}-claude/03-itinerary.md`, `04-budget.md`)을 읽는다.
3. Codex 결과셋(`artifacts/{run-id}-codex/03-itinerary.md`, `04-budget.md`)을 읽는다.
4. 각 항목별로 두 결과를 채점한다. 채점 근거를 한 줄씩 명시한다.
5. 총점을 계산한다.
   - 총점 차이 5점 이상: 높은 점수 안을 권고한다.
   - 총점 차이 5점 미만: 항목별 강점을 분석해 조건부 권고를 한다.
6. 하이브리드 가능 여부를 검토한다. (A안 일정 + B안 예산처럼 부분 채택)
7. 사용자에게 최종안 채택 승인을 요청한다.
8. `harness/schemas/comparison.schema.md` 형식에 따라 `artifacts/comparison-{run-id}.md`로 저장한다.
