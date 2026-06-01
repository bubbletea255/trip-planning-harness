# Harness Manifest

각 Phase에서 어떤 파일이 어떤 역할을 하는지 한눈에 보여주는 지도.
파일을 수정할 때 이 표를 기준으로 어디를 고쳐야 하는지 확인한다.

---

## 파일 역할 지도

| Phase | Contract | Procedure | Schema (출력) | Claude 어댑터 | Codex 어댑터 | 산출물 |
|---|---|---|---|---|---|---|
| 1 Intake | contracts/intake.contract.md | procedures/intake.md | schemas/intake.schema.md | .claude/agents/trip-intake.md | .agents/skills/trip-intake/SKILL.md | 01-intake.md |
| 2 Research | contracts/researcher.contract.md | procedures/researcher.md | schemas/research.schema.md | .claude/agents/trip-researcher.md | .agents/skills/trip-researcher/SKILL.md | 02-research.md |
| 3 Planning | contracts/planner.contract.md | procedures/planner.md | schemas/itinerary.schema.md | .claude/agents/trip-planner.md | .agents/skills/trip-planner/SKILL.md | 03-itinerary.md |
| 4 Budget | contracts/budget-analyst.contract.md | procedures/budget-analyst.md | schemas/budget.schema.md | .claude/agents/trip-budget-analyst.md | .agents/skills/trip-budget-analyst/SKILL.md | 04-budget.md |
| 5 Report | contracts/report-composer.contract.md | procedures/report-composer.md | schemas/report.schema.md | .claude/agents/trip-report-composer.md | .agents/skills/trip-report-composer/SKILL.md | 05-report.html |
| 6 Judge | contracts/judge.contract.md | procedures/judge.md | schemas/comparison.schema.md | .claude/agents/trip-judge.md | .agents/skills/trip-judge/SKILL.md | comparison-{run-id}.md |

## 오케스트레이터 어댑터

| 플랫폼 | 파일 | 역할 |
|---|---|---|
| Codex | .agents/skills/trip-planning-orchestrator/SKILL.md | 자연어 여행 요청을 받아 `harness/ORCHESTRATOR.md` 흐름으로 라우팅 |
| Claude Code | CLAUDE.md + .claude/agents/*.md | 프로젝트 안내와 Phase별 sub-agent 호출 |

## 공통 Schema

| 파일 | 용도 |
|---|---|
| schemas/intake-questionnaire.schema.md | Intake가 수집 여부를 판단하는 필수/선택 항목 기준표 |

## 평가 기준

| 파일 | 용도 |
|---|---|
| rubrics/judge.rubric.md | Judge가 채점할 때 사용하는 5항목 루브릭 |

---

## 수정 원칙

| 바꾸고 싶은 것 | 수정 위치 |
|---|---|
| 단계 목적·입출력·금지사항 | `harness/contracts/` |
| 수행 절차·방법론 | `harness/procedures/` |
| 출력 파일 형식 | `harness/schemas/` |
| 평가 기준 | `harness/rubrics/` |
| 전체 흐름·Phase 순서 | `harness/ORCHESTRATOR.md` |
| Claude sub-agent 호출 조건·도구·모델 | `.claude/agents/` |
| Codex skill 트리거·실행 방식 | `.agents/skills/` |
| Codex 모델·실행 환경 | `.codex/config.toml` |

---

## 버전 관리

| 날짜 | 변경 내용 |
|---|---|
| 2026-05-31 | v3: contract+adapter 구조로 전면 재설계 |
| 2026-05-31 | v3.1: Codex 어댑터 7개와 .codex/config.toml 추가 |
| 2026-06-01 | v3.2: Claude/Codex 정합성 패치. 예비비, Intake, 웹 조사, 숙박비 기준을 공통 원장으로 통일하고 Codex Skill을 한국어 중심으로 정리 |
