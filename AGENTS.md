# 여행 계획 하네스 v2

여행 조건을 입력하면 일정표, 예산표, HTML 리포트를 생성한다.
Claude Code와 Codex 양쪽에서 동일하게 작동한다.

---

## 하네스 구조

```
trip-planning-harness/
├── orchestrator/ORCHESTRATOR.md  ← 전체 실행 흐름 (Phase 0-7)
├── agents/                       ← 에이전트 역할 정의 (WHO)
├── skills/                       ← 에이전트 실행 지침 (HOW)
├── prompts/                      ← 공유 템플릿 (WHAT)
└── artifacts/                    ← 실행 결과물
```

---

## 자연어 라우팅

아래 표현이 포함된 요청은 `orchestrator/ORCHESTRATOR.md`를 읽고 실행한다.

| 요청 유형 | 예시 표현 |
|---|---|
| 새 여행 계획 | "도쿄 여행 계획 짜줘", "제주 3박 4일 일정", "[목적지] 여행" |
| 직접 호출 | `/trip-planning` |
| 부분 수정 | "숙박만 바꿔줘", "예산 다시 계산해줘", "일정 여유롭게 수정해줘" |
| 교차 검증 | "두 결과 비교해줘", "Claude Codex 비교해줘" |
| 재실행 | "처음부터 다시", "이전 계획 업데이트해줘" |

---

## 에이전트 팀

각 에이전트는 역할 파일(agents/)과 지침 파일(skills/)을 함께 읽고 수행한다.

| 에이전트 | 역할 파일 | 지침 파일 | 활성화 조건 |
|---|---|---|---|
| intake | agents/intake.md | skills/intake.md | Phase 1 |
| researcher | agents/researcher.md | skills/researcher.md | 01-intake.md 완료 후 |
| planner | agents/planner.md | skills/planner.md | 02-research.md 완료 후 |
| budget-analyst | agents/budget-analyst.md | skills/budget-analyst.md | 03-itinerary.md 완료 후 |
| report-composer | agents/report-composer.md | skills/report-composer.md | 03, 04 완료 후 |
| judge | agents/judge.md | skills/judge.md | 비교 모드에서만 |

---

## 수정 원칙

- 에이전트 역할 변경 → `agents/` 파일 편집
- 실행 지침 변경 → `skills/` 파일 편집
- 흐름 변경 → `orchestrator/ORCHESTRATOR.md` 편집
- **AGENTS.md를 수정하면 CLAUDE.md에도 동일하게 반영한다**

---

## 변경 이력

| 날짜 | 내용 |
|---|---|
| 2026-05-31 | v1: 최초 구성 (core/ + .claude/ 이중 구조) |
| 2026-05-31 | v2: 평탄 단일 구조로 전면 재설계 (agents/skills/prompts 3계층) |
