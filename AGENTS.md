# 여행 계획 하네스 v3

여행 조건을 입력하면 일정표, 예산표, HTML 리포트를 생성한다.
Claude Code와 Codex 양쪽에서 동일하게 작동한다.

---

## 하네스 구조

```
trip-planning-harness/
├── harness/                      ← 공통 업무 원장 (플랫폼 중립)
│   ├── ORCHESTRATOR.md           ← 전체 실행 흐름 (Phase 0-7)
│   ├── MANIFEST.md               ← 파일 역할 지도
│   ├── contracts/                ← 목적·입출력·완료기준·금지사항 (6개)
│   ├── procedures/               ← 수행 절차·방법론 (6개)
│   ├── schemas/                  ← 산출물 형식 템플릿 (7개)
│   └── rubrics/                  ← 평가 기준 (1개)
├── .claude/agents/               ← Claude Code 실행 어댑터 (2차 완료 — 6개)
├── .agents/skills/               ← Codex 실행 어댑터 (3차 완료 — orchestrator + 6개)
├── .codex/config.toml            ← Codex 실행 설정 (3차 완료 — 최소 설정)
└── artifacts/                    ← 실행 결과물
```

---

## 자연어 라우팅

아래 표현이 포함된 요청은 `harness/ORCHESTRATOR.md`를 읽고 실행한다.
Codex 환경에서는 `.agents/skills/trip-planning-orchestrator/SKILL.md`를 우선 진입점으로 사용한다.

| 요청 유형 | 예시 표현 |
|---|---|
| 새 여행 계획 | "도쿄 여행 계획 짜줘", "제주 3박 4일 일정", "[목적지] 여행" |
| 직접 호출 | `/trip-planning` |
| 부분 수정 | "숙박만 바꿔줘", "예산 다시 계산해줘", "일정 여유롭게 수정해줘" |
| 교차 검증 | "두 결과 비교해줘", "Claude Codex 비교해줘" |
| 재실행 | "처음부터 다시", "이전 계획 업데이트해줘" |

---

## 에이전트 팀

각 Phase에서 contract(목적·기준)와 procedure(절차)를 함께 읽고 수행한다.

| Phase | Contract | Procedure | 활성화 조건 |
|---|---|---|---|
| 1 Intake | harness/contracts/intake.contract.md | harness/procedures/intake.md | Phase 1 |
| 2 Research | harness/contracts/researcher.contract.md | harness/procedures/researcher.md | 01-intake.md 완료 후 |
| 3 Planning | harness/contracts/planner.contract.md | harness/procedures/planner.md | 02-research.md 완료 후 |
| 4 Budget | harness/contracts/budget-analyst.contract.md | harness/procedures/budget-analyst.md | 03-itinerary.md 완료 후 |
| 5 Report | harness/contracts/report-composer.contract.md | harness/procedures/report-composer.md | 03, 04 완료 후 |
| 6 Judge | harness/contracts/judge.contract.md | harness/procedures/judge.md | 비교 모드에서만 |

---

## 수정 원칙

- 업무 의미 변경 → `harness/contracts/` 파일 편집
- 수행 절차 변경 → `harness/procedures/` 파일 편집
- 출력 형식 변경 → `harness/schemas/` 파일 편집
- 전체 흐름 변경 → `harness/ORCHESTRATOR.md` 편집
- Claude 실행 방식 변경 → `.claude/agents/` 파일 편집
- Codex 실행 방식 변경 → `.agents/skills/` 파일 편집
- **AGENTS.md와 CLAUDE.md 중 하나를 수정하면 다른 파일에도 동일하게 반영한다**

---

## 변경 이력

| 날짜 | 내용 |
|---|---|
| 2026-05-31 | v1: 최초 구성 (core/ + .claude/ 이중 구조) |
| 2026-05-31 | v2: 평탄 단일 구조로 전면 재설계 (agents/skills/prompts 3계층) |
| 2026-05-31 | v3: contract+adapter 구조로 전면 재설계 (harness/ 공통 원장 + 플랫폼 어댑터 분리) |
| 2026-05-31 | v3.1: Codex 어댑터 구성 (.agents/skills orchestrator+6개, .codex/config.toml) |
| 2026-06-01 | v3.2: Claude/Codex 정합성 패치 (예비비·Intake·웹 조사·숙박비 기준 통일, Codex Skill 한국어화) |
