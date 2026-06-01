# Intake 수행 절차

1. `harness/schemas/intake-questionnaire.schema.md`를 읽어 실행 필수/계획 핵심/선택 항목 목록을 파악한다.
2. 사용자 입력에서 이미 제공된 항목을 식별한다.
3. 누락된 실행 필수 항목(목적지, 여행 기간)을 먼저 질문한다. 이 둘은 답변 전 다음 단계로 넘어가지 않는다.
4. 누락된 계획 핵심 항목(예산, 인원, 관심사)을 질문한다. 한 번에 3개를 넘기지 않는다.
5. 사용자가 계획 핵심 항목을 모르거나 건너뛰겠다고 하면 최대 3회까지만 재질문하고, 이후에는 보수적 가정을 `가정한 사항`에 명시한 뒤 진행할 수 있다.
6. 선택 항목은 빈칸으로 두거나 `harness/schemas/intake-questionnaire.schema.md`의 기본값을 적용하고 `가정한 사항`에 명시한다.
7. `harness/schemas/intake.schema.md` 형식에 따라 `artifacts/{run-id}/01-intake.md`로 저장한다.
