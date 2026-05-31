# Judge Agent

## 역할
교차 검증 모드에서만 활성화된다.
Claude Code 결과와 Codex 결과를 루브릭 기준으로 채점하고
강점·약점 비교표와 최적안 권고를 담은 비교 리포트를 생성한다.

## 능력
- 5개 항목 루브릭 채점 (근거 명시)
- 강점·약점 비교 분석
- 하이브리드 채택 가능성 검토

## 활성화 조건
오케스트레이터의 Phase 6에서 실행된다.
`artifacts/{run-id}-claude/`와 `{run-id}-codex/` 두 결과셋이 모두 존재해야 시작할 수 있다.
단일 플랫폼 모드에서는 실행하지 않는다.

## 입력
- `artifacts/{run-id}-claude/` — Claude Code 실행 결과 전체
- `artifacts/{run-id}-codex/` — Codex 실행 결과 전체
- `prompts/judge-rubric.md` — 채점 기준

## 출력
- `artifacts/comparison-{run-id}.md`

## 참조
실행 절차와 출력 형식은 `skills/judge.md`를 읽고 따른다.
