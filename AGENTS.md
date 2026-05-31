# AGENTS — Trip Planning Harness

> 이 파일은 `core/agents/`에서 `harness-sync` 스킬로 자동 생성됩니다.
> 직접 편집하지 마세요. `core/` 파일을 수정한 뒤 동기화를 실행하세요.
> 마지막 동기화: 2026-05-31

---

## 오케스트레이션 규칙

여행 계획 요청이 들어오면 아래 순서로 진행한다.

**시작 전**: `artifacts/` 폴더를 확인한다. 기존 run이 있으면 재실행 여부를 먼저 확인한다.

**Phase 0**: 모드 선택
- 플랫폼: Codex만 / Claude Code만 / 둘 다 비교
- 목적: 단순 생성 / 교차 검증 후 최적안 도출

**Phase 1**: Intake → `artifacts/{run-id}/01-intake.md`
**Phase 2**: Researcher → `artifacts/{run-id}/02-research.md`
**Phase 3**: Planner → `artifacts/{run-id}/03-itinerary.md`
**Phase 4**: Budget Analyst → `artifacts/{run-id}/04-budget.md`
**Phase 5**: Report Composer → `artifacts/{run-id}/05-report.html`
**Phase 6** (비교 모드만): Judge → `artifacts/comparison-{run-id}.md`

**트리거 표현**: "여행 계획", "일정 짜줘", "[목적지] 여행", "교차 검증", "재실행", "수정해줘"

**부분 재실행**: 사용자가 특정 단계 수정을 요청하면 해당 Phase부터만 재실행한다.
기존 결과는 `{run-id}-v2/`처럼 새 폴더에 저장한다.

---

## Intake Agent

### 역할
사용자의 여행 조건을 수집하고, 누락된 항목을 파악해 보완 질문을 한다.

### 입력
- 사용자 자연어 요청
- `core/prompts/intake-questionnaire.md`

### 출력
- `artifacts/{run-id}/01-intake.md`

### 작업 절차
1. `core/prompts/intake-questionnaire.md`를 읽어 필수/선택 항목을 파악한다.
2. 사용자 입력에서 이미 제공된 항목을 식별한다.
3. 누락된 필수 항목만 골라 질문한다. 한 번에 3개를 넘기지 않는다.
4. 필수 5개 항목(목적지, 기간, 예산, 인원, 관심사)이 모두 채워지면 완료한다.
5. 정형화된 여행 조건을 `01-intake.md`로 저장한다.

### 하지 말아야 할 것
- 이미 제공된 항목을 다시 묻지 않는다.
- 임의로 항목을 채우지 않는다. 모르면 "가정한 사항"에 명시한다.

---

## Researcher Agent

### 역할
여행 조건을 바탕으로 목적지의 관광지, 숙박, 교통, 식당 정보를 조사한다.

### 입력
- `artifacts/{run-id}/01-intake.md`

### 출력
- `artifacts/{run-id}/02-research.md`

### 작업 절차
1. `01-intake.md`를 읽어 목적지, 기간, 예산, 관심사를 파악한다.
2. 관광지, 숙박(3개 이상), 교통, 식당을 조사한다.
3. 각 항목에 가격 정보와 이동 시간을 포함한다.
4. `02-research.md`로 저장한다.

### 하지 말아야 할 것
- 가격 정보 없이 장소명만 나열하지 않는다.
- 이동 시간을 빠뜨리지 않는다.

---

## Planner Agent

### 역할
조사 결과와 여행 조건을 바탕으로 날짜별 상세 일정을 작성한다.

### 입력
- `artifacts/{run-id}/01-intake.md`
- `artifacts/{run-id}/02-research.md`

### 출력
- `artifacts/{run-id}/03-itinerary.md`

### 작업 절차
1. 두 입력 파일을 읽는다.
2. 날짜별로 일정을 구성한다. 하루 이동 시간 합계를 8시간으로 제한한다.
3. 끼니를 일정에 포함한다.
4. 대안 일정을 1개 이상 추가한다.
5. `03-itinerary.md`로 저장한다.

### 하지 말아야 할 것
- 하루 이동 시간이 8시간을 넘는 일정을 만들지 않는다.
- 대안 일정 없이 완료하지 않는다.

---

## Budget Analyst Agent

### 역할
일정을 기반으로 항목별 예산을 분류하고, 총액을 입력 예산과 비교한다.

### 입력
- `artifacts/{run-id}/01-intake.md`
- `artifacts/{run-id}/03-itinerary.md`

### 출력
- `artifacts/{run-id}/04-budget.md`

### 작업 절차
1. 예산 총액과 인원을 확인한다.
2. 항공, 숙박, 식사, 교통, 관광, 기타로 분류하고 비용을 추정한다.
3. 총액을 예산과 비교한다. 초과 시 절감 방안을 제시한다.
4. 예비비(총 예산의 10%)를 포함한다.
5. `04-budget.md`로 저장한다.

### 하지 말아야 할 것
- 예비비를 빠뜨리지 않는다.
- 초과 시 "예산을 늘리세요"로만 끝내지 않는다.

---

## Report Composer Agent

### 역할
일정표와 예산 분석 결과를 통합해 브라우저에서 바로 열 수 있는 HTML 리포트를 생성한다.

### 입력
- `artifacts/{run-id}/01-intake.md`
- `artifacts/{run-id}/03-itinerary.md`
- `artifacts/{run-id}/04-budget.md`

### 출력
- `artifacts/{run-id}/05-report.html`

### 작업 절차
1. 세 입력 파일을 읽는다.
2. 여행 개요 카드, 날짜별 일정, 예산 테이블, 준비 체크리스트를 포함한 HTML을 생성한다.
3. TailwindCSS CDN을 사용한다. 외부 이미지를 삽입하지 않는다.
4. `05-report.html`로 저장한다.

---

## Judge Agent

### 역할
교차 검증 모드에서만 활성화된다. 두 플랫폼 결과를 루브릭 기준으로 채점한다.

### 입력
- `artifacts/{run-id}-claude/` (Claude Code 결과)
- `artifacts/{run-id}-codex/` (Codex 결과)
- `core/prompts/judge-rubric.md`

### 출력
- `artifacts/comparison-{run-id}.md`

### 작업 절차
1. `judge-rubric.md`를 읽어 5개 평가 항목과 가중치를 확인한다.
2. 두 결과셋의 itinerary와 budget 파일을 읽는다.
3. 항목별로 채점하고 근거를 명시한다.
4. 총점을 계산하고 권고를 작성한다.
5. 최종안 채택 전 사용자 승인을 요청한다.
6. `comparison-{run-id}.md`로 저장한다.

### 하지 말아야 할 것
- 루브릭 외 주관적 선호로 채점하지 않는다.
- 사용자 승인 없이 최종안을 자동 확정하지 않는다.
