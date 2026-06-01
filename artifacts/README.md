# Artifacts — 산출물 지도

이 폴더는 여행 계획 하네스의 모든 실행 결과를 저장한다.
새 실행마다 `{run-id}/` 폴더가 생성된다.

---

## 폴더 구조

```
artifacts/
├── README.md                    ← 이 파일 (산출물 지도)
├── improvement-log.md           ← 실행 피드백 및 하네스 개선 기록
│
├── {run-id}/                    ← 단일 모드 실행 결과
│   ├── 01-intake.md             ← Intake Agent 출력 (여행 조건)
│   ├── 02-research.md           ← Researcher Agent 출력 (조사 결과)
│   ├── 03-itinerary.md          ← Planner Agent 출력 (날짜별 일정)
│   ├── 04-budget.md             ← Budget Analyst Agent 출력 (예산 분석)
│   └── 05-report.html           ← Report Composer Agent 출력 (최종 리포트)
│
├── {run-id}-claude/             ← 비교 모드: Claude Code 실행 결과
├── {run-id}-codex/              ← 비교 모드: Codex 실행 결과
└── comparison-{run-id}.md       ← Judge Agent 출력 (비교 리포트)
```

---

## run-id 형식

`run-YYYYMMDD-{목적지약어}`

예시:
- `run-20260531-seoul` (서울 여행, 단일 모드)
- `run-20260531-tokyo-claude` (도쿄 여행, 비교 모드 Claude 결과)
- `run-20260531-tokyo-codex` (도쿄 여행, 비교 모드 Codex 결과)

---

## 파일 의존 관계

```
01-intake.md
    ↓ (Researcher 읽음)
02-research.md
    ↓ (Planner 읽음)
03-itinerary.md
    ↓ (Budget Analyst, Report Composer 읽음)
04-budget.md
    ↓ (Report Composer 읽음)
05-report.html  ← 사용자가 브라우저에서 열기

(비교 모드)
{run-id}-claude/ + {run-id}-codex/
    ↓ (Judge 읽음)
comparison-{run-id}.md  ← 사용자가 최종안 선택
```

---

## 재실행 규칙

부분 수정 시 기존 결과를 덮어쓰지 않는다.
새 폴더(`{run-id}-v2/`)에 저장한다.

---

## 현재 실행 목록

| run-id | 목적지 | 날짜 | 모드 | 상태 |
|---|---|---|---|---|
| run-20260531-seoul | 서울 | 2026-05-31 | 단일 (Claude Code) | ✓ 완료 (v1) |
| run-20260531-seoul-v2 | 서울 | 2026-05-31 | 단일 (Codex) | ✓ 완료 (v1) |
| run-20260531-seoul-v3 | 서울 | 2026-05-31 | 단일 (Claude Code) | ✓ 완료 (v2 구조 검증) |
| run-20260531-seoul-v4 | 서울 | 2026-05-31 | 단일 (Codex) | ✓ 완료 (가족 여행 기본안) |
| run-20260531-seoul-v5 | 서울 | 2026-05-31 | 단일 (Claude Code) | ✓ 완료 (v3 구조 스모크 테스트) |
