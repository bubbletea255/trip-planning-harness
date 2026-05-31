---
name: harness-sync
description: Synchronizes core/ agent definitions to platform-specific adapters. Activate when: "어댑터 동기화해줘", "Codex 파일 업데이트해줘", "core 바꿨는데 동기화해줘", "/harness-sync", "플랫폼 파일 재생성해줘". Run this after modifying any file in core/agents/ or core/prompts/ to keep .claude/agents/ and AGENTS.md in sync.
---

# Harness Sync Skill

## 역할
`core/` 폴더의 Agent 정의를 Claude Code와 Codex 플랫폼 형식으로 동기화한다.
`core/`는 단일 진실 소스다. 플랫폼 파일을 직접 편집하지 말고 이 스킬로 동기화한다.

---

## 동기화 대상

| 소스 | 대상 | 변환 내용 |
|---|---|---|
| `core/agents/*.md` | `.claude/agents/*.md` | Claude frontmatter 추가 |
| `core/agents/*.md` | `AGENTS.md` | Codex 단일 파일 형식으로 통합 |

---

## 작업 절차

### Step 1: core/ 변경 내용 파악

```
읽기: core/agents/ 폴더의 모든 .md 파일 목록
- intake.md
- researcher.md
- planner.md
- budget-analyst.md
- report-composer.md
- judge.md
```

### Step 2: Claude Code 어댑터 동기화 (.claude/agents/)

각 core/agents/{name}.md에 대해:

1. 파일 내용을 읽는다.
2. 해당 Agent의 Claude frontmatter 템플릿을 적용한다.
3. `.claude/agents/{name}.md`로 덮어쓴다.

**Claude frontmatter 템플릿**:
```markdown
---
name: {agent-name}
description: {core 파일의 "## 역할" 첫 문장을 영어로 요약. 언제 활성화하고 언제 하지 말아야 할지 포함.}
---

{core 파일 전체 내용}

---
*canonical: core/agents/{name}.md*
```

### Step 3: Codex 어댑터 동기화 (AGENTS.md)

`AGENTS.md`를 아래 형식으로 새로 생성한다.

```markdown
# AGENTS — Trip Planning Harness

이 파일은 core/agents/에서 자동 생성됩니다. 직접 편집하지 마세요.
마지막 동기화: {날짜}

---

## 오케스트레이션 규칙

여행 계획 요청이 들어오면 아래 순서로 진행한다.

**Phase 0**: 모드 선택 (플랫폼, 단일/비교)
**Phase 1**: Intake Agent → artifacts/{run-id}/01-intake.md
**Phase 2**: Researcher Agent → artifacts/{run-id}/02-research.md
**Phase 3**: Planner Agent → artifacts/{run-id}/03-itinerary.md
**Phase 4**: Budget Analyst Agent → artifacts/{run-id}/04-budget.md
**Phase 5**: Report Composer Agent → artifacts/{run-id}/05-report.html
**Phase 6** (비교 모드): Judge Agent → artifacts/comparison-{run-id}.md

트리거 표현: "여행 계획", "일정 짜줘", "[목적지] 여행", "교차 검증"

---

## Intake Agent

{core/agents/intake.md 전체 내용}

---

## Researcher Agent

{core/agents/researcher.md 전체 내용}

---

## Planner Agent

{core/agents/planner.md 전체 내용}

---

## Budget Analyst Agent

{core/agents/budget-analyst.md 전체 내용}

---

## Report Composer Agent

{core/agents/report-composer.md 전체 내용}

---

## Judge Agent

{core/agents/judge.md 전체 내용}
```

### Step 4: 동기화 완료 보고

```
동기화 완료 보고 형식:
  ✓ .claude/agents/intake.md
  ✓ .claude/agents/researcher.md
  ✓ .claude/agents/planner.md
  ✓ .claude/agents/budget-analyst.md
  ✓ .claude/agents/report-composer.md
  ✓ .claude/agents/judge.md
  ✓ AGENTS.md (Codex 어댑터)

  동기화된 Agent 수: 6
  마지막 동기화: {날짜 시간}
```

---

## 주의사항

- `.claude/agents/` 파일을 직접 편집했다면 `core/`에 먼저 반영한 뒤 동기화한다.
- `AGENTS.md`는 자동 생성 파일이다. 직접 편집하지 않는다.
- 동기화 후 `improvement-log.md`에 변경 내용을 기록한다.

---

## 검증 체크리스트

동기화 후 아래를 확인한다.
- [ ] core/agents/ 파일 수 = .claude/agents/ 파일 수
- [ ] AGENTS.md에 6개 Agent 섹션 모두 포함됨
- [ ] 각 .claude/agents/ 파일에 frontmatter가 있음
- [ ] AGENTS.md 상단에 오케스트레이션 규칙이 있음
