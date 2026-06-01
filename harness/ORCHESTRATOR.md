# Trip Planning Orchestrator

## 역할
여행 계획 하네스의 전체 흐름을 관리한다.
모드 선택 → 입력 수집 → 조사 → 일정 → 예산 → 리포트 → (교차 검증) 순서로 진행한다.

각 단계에서 해당 단계의 contract 파일(`harness/contracts/`)과 procedure 파일(`harness/procedures/`)을 읽고 수행한다.

---

## 수정 경계 (Modification Boundaries)

- 업무 의미(목적·입력·출력·기준·금지)는 `harness/contracts/`에서만 변경한다.
- 수행 절차는 `harness/procedures/`에서만 변경한다.
- 출력 형식은 `harness/schemas/`에서만 변경한다.
- 평가 기준은 `harness/rubrics/`에서만 변경한다.
- Claude 실행 방식은 `.claude/agents/`에서만 변경한다.
- Codex 실행 방식은 `.agents/skills/`와 `.codex/`에서만 변경한다.

---

## 시작 전 확인: 실행 유형 판단

`artifacts/` 폴더를 확인한다.

| 상황 | 처리 방식 |
|---|---|
| `artifacts/` 없음 또는 비어 있음 | 초기 실행 → Phase 0부터 전체 진행 |
| `artifacts/{run-id}/` 존재 | 재실행 여부 확인 → 부분 수정 요청이면 해당 Phase만 재실행 |
| 비교 모드 결과 (`*-claude/`, `*-codex/`) 존재 | Judge 단독 실행 가능 |

---

## Phase 0: 모드 선택 가이드

사용자에게 두 가지를 확인한다. (이미 명확하면 생략)

**질문 1 — 플랫폼**
> 이번 여행 계획을 어떤 방식으로 만들어드릴까요?
> A) Claude Code만 사용 (빠른 단일 결과)
> B) Codex만 사용 (Codex 환경에서 실행)
> C) 둘 다 실행하고 나중에 비교 (교차 검증 모드)

**질문 2 — 목적** (C를 선택한 경우)
> 비교 후 어떻게 활용할 계획인가요?
> A) 단순 참고용 비교
> B) Judge가 루브릭 기준으로 채점하고 최적안을 권고

→ 선택 결과를 run-id에 반영한다.
- 단일 모드: `run-YYYYMMDD-{목적지약어}`
- 비교 모드: `run-YYYYMMDD-{목적지약어}-claude` / `run-YYYYMMDD-{목적지약어}-codex`

---

## Phase 1: 입력 수집 (Intake)

`harness/contracts/intake.contract.md`를 읽어 목적과 제약을 파악하고,
`harness/procedures/intake.md`를 읽어 절차를 따른다.

```
목표: artifacts/{run-id}/01-intake.md 생성
완료 조건: 목적지·기간 수집됨 + 예산·인원·관심사는 수집 또는 명시적 가정 처리됨
실패 시: 목적지·기간이 없으면 중단하고 재질문. 예산·인원·관심사는 3회 시도 후에도 미수집이면 "가정한 사항"에 표시하고 진행 가능
```

---

## Phase 2: 조사 (Researcher)

`01-intake.md`가 존재하는지 확인한 뒤,
`harness/contracts/researcher.contract.md`와 `harness/procedures/researcher.md`를 읽고 수행한다.

```
목표: artifacts/{run-id}/02-research.md 생성
완료 조건: 관광지, 숙박, 교통, 식당 4개 카테고리 모두 포함 + 가격 정보 포함
실패 시: 부족한 카테고리만 재조사
```

---

## Phase 3: 일정 작성 (Planner)

`01-intake.md`와 `02-research.md`가 모두 존재하는지 확인한 뒤,
`harness/contracts/planner.contract.md`와 `harness/procedures/planner.md`를 읽고 수행한다.

```
목표: artifacts/{run-id}/03-itinerary.md 생성
완료 조건: 요청 일수 전체 계획됨 + 대안 일정 1개 이상 + 하루 이동시간 명시
실패 시: 문제 있는 날짜만 재작성
```

---

## Phase 4: 예산 분석 (Budget Analyst)

`03-itinerary.md`가 존재하는지 확인한 뒤,
`harness/contracts/budget-analyst.contract.md`와 `harness/procedures/budget-analyst.md`를 읽고 수행한다.

```
목표: artifacts/{run-id}/04-budget.md 생성
완료 조건: 6개 카테고리 분류 + 입력 예산 기준 예비비 10% 포함 + 예산 초과 시 절감 방안 포함
실패 시: 누락된 카테고리 보완
```

---

## Phase 5: HTML 리포트 생성 (Report Composer)

`03-itinerary.md`와 `04-budget.md`가 모두 존재하는지 확인한 뒤,
`harness/contracts/report-composer.contract.md`와 `harness/procedures/report-composer.md`를 읽고 수행한다.

```
목표: artifacts/{run-id}/05-report.html 생성
완료 조건: 4개 섹션(개요, 일정, 예산, 체크리스트) 모두 포함 + 브라우저에서 열림
```

---

## Phase 6: 교차 검증 (Judge) — 비교 모드만

두 플랫폼 실행이 모두 완료된 경우에만 진행한다.
`{run-id}-claude/05-report.html`과 `{run-id}-codex/05-report.html`이 모두 존재하는지 확인한다.

`harness/contracts/judge.contract.md`와 `harness/procedures/judge.md`를 읽고 수행한다.

```
목표: artifacts/comparison-{run-id}.md 생성
완료 조건: 5개 항목 채점 + 근거 명시 + 권고 포함
사람 승인 필요: 최종안 채택 전 사용자 확인
```

---

## Phase 7: 마무리

```
1. artifacts/README.md를 업데이트한다.
2. artifacts/improvement-log.md에 실행 결과 요약을 추가한다.
3. 사용자에게 결과물 위치를 안내한다.

안내 형식:
  ✓ 완료: artifacts/{run-id}/
    - 05-report.html  → 브라우저에서 열기
    - 03-itinerary.md → 일정표 확인
    - 04-budget.md    → 예산 상세
    (비교 모드) comparison-{run-id}.md → 비교 리포트
```

---

## 부분 재실행 규칙

| 사용자 요청 | 재실행 Phase |
|---|---|
| "숙박만 바꿔줘", "숙박 더 저렴하게" | Phase 2 → 4 → 5 |
| "예산 다시 계산해줘" | Phase 4 → 5 |
| "일정 여유롭게 바꿔줘" | Phase 3 → 4 → 5 |
| "리포트만 다시 만들어줘" | Phase 5만 |
| "처음부터 다시" | Phase 1부터 전체 |

재실행 시 기존 결과는 덮어쓰지 않고 `{run-id}-v2/`처럼 새 폴더에 저장한다.

---

## 오류 처리

| 상황 | 대응 |
|---|---|
| 웹 조사 실패 | 알려진 정보로 대체하고 "확인 필요" 표시 |
| 예산 대폭 초과 (30% 이상) | Budget Analyst에게 절감 방안 2개 이상 요구 후 사용자에게 선택 요청 |
| 이동 시간 8시간 초과 감지 | Planner에게 해당 날짜 재작성 요청 |
| Judge 동점 (차이 0점) | 항목별 강점 비교로 조건부 권고 작성 |
