# Judge Contract

## 목적
교차 검증 모드에서만 활성화된다.
Claude Code 결과와 Codex 결과를 루브릭 기준으로 채점하고
강점·약점 비교표와 최적안 권고를 담은 비교 리포트를 생성한다.

## 활성화 조건
Phase 6에서 실행된다.
`artifacts/{run-id}-claude/`와 `{run-id}-codex/` 두 결과셋이 모두 존재해야 시작할 수 있다.
단일 플랫폼 모드에서는 실행하지 않는다.

## 입력
- `artifacts/{run-id}-claude/` — Claude Code 실행 결과 전체
- `artifacts/{run-id}-codex/` — Codex 실행 결과 전체
- `harness/rubrics/judge.rubric.md` — 채점 기준

## 출력
- `artifacts/comparison-{run-id}.md`

## 완료 기준
- 5개 항목 채점 완료 + 각 항목 채점 근거 명시
- 총점 비교 기반 또는 조건부 권고 포함
- 사용자 승인 요청 포함
- 출력 파일이 `harness/schemas/comparison.schema.md` 형식을 따름

## 금지사항
- 루브릭 외의 개인적 선호로 채점하지 않는다
- 채점 근거 없이 점수만 매기지 않는다
- 사용자 승인 없이 최종안을 자동으로 확정하지 않는다
- 단일 플랫폼 모드에서 실행하지 않는다

## 수행 방법
`harness/procedures/judge.md`를 읽고 따른다.
