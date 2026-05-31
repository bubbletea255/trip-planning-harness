# 여행 계획 하네스 (Trip Planning Harness)

Claude Code와 Codex 양쪽에서 동작하는 여행 계획 생성 하네스.
여행 조건을 입력하면 일정표, 예산표, HTML 리포트를 생성한다.
선택적으로 두 플랫폼 결과를 Judge Agent가 루브릭 기준으로 비교한다.

---

## 자연어 라우팅 규칙

아래 표현이 포함된 요청은 `trip-planning-orchestrator`를 먼저 실행한다.

| 요청 유형 | 예시 표현 |
|---|---|
| 새 여행 계획 | "도쿄 여행 계획 짜줘", "제주 3박 4일 일정", "파리 여행 만들어줘" |
| 직접 호출 | `/trip-planning` |
| 부분 수정 | "숙박만 바꿔줘", "예산 다시 계산해줘", "일정 여유롭게 수정해줘" |
| 교차 검증 | "Claude랑 Codex 비교해줘", "두 결과 비교해줘", "교차 검증 해줘" |
| 재실행 | "처음부터 다시", "이전 여행 계획 업데이트해줘" |

어댑터 동기화는 `harness-sync`가 담당한다.

| 요청 유형 | 예시 표현 |
|---|---|
| 동기화 | "어댑터 동기화해줘", "Codex 파일 업데이트해줘", `/harness-sync` |

---

## 하네스 구조

```
trip-planning-harness/
├── core/                          ← 단일 진실 소스 (플랫폼 무관)
│   ├── agents/                    ← 6개 Agent 역할 정의
│   └── prompts/                   ← intake-questionnaire.md, judge-rubric.md
│
├── .claude/                       ← Claude Code 어댑터
│   ├── agents/                    ← core + Claude frontmatter
│   ├── skills/
│   │   ├── trip-planning-orchestrator/  ← 메인 진입점
│   │   └── harness-sync/               ← 플랫폼 동기화
│   └── CLAUDE.md                  ← 이 파일
│
├── AGENTS.md                      ← Codex 어댑터 (harness-sync로 생성)
│
└── artifacts/                     ← 실행 결과물
    ├── README.md                  ← 산출물 지도
    ├── {run-id}/                  ← 실행마다 새 폴더
    │   ├── 01-intake.md
    │   ├── 02-research.md
    │   ├── 03-itinerary.md
    │   ├── 04-budget.md
    │   └── 05-report.html         ← 브라우저에서 열기
    └── improvement-log.md
```

---

## Agent 팀

| Agent | 역할 | 활성화 조건 |
|---|---|---|
| intake | 여행 조건 수집 | 첫 번째 단계 |
| researcher | 목적지 조사 | 01-intake.md 완료 후 |
| planner | 일정 작성 | 02-research.md 완료 후 |
| budget-analyst | 예산 분석 | 03-itinerary.md 완료 후 |
| report-composer | HTML 리포트 생성 | 03, 04 완료 후 |
| judge | 교차 검증 채점 | 비교 모드에서 두 결과 모두 완료 후 |

---

## 플랫폼 동기화 원칙

- `core/`를 수정했으면 반드시 동기화를 실행한다.
- `.claude/agents/`와 `AGENTS.md`를 직접 편집하지 않는다.
- 직접 편집이 필요하면 `core/`에 먼저 반영한 뒤 동기화한다.

---

## 변경 이력

| 날짜 | 변경 내용 | 대상 |
|---|---|---|
| 2026-05-31 | 최초 구성 | 전체 |
