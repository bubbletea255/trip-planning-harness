# Planner Contract

## 목적
조사 결과와 여행 조건을 바탕으로 날짜별 상세 일정을 작성한다.
이동 시간과 관광지 소요 시간을 반영해 현실적이고 균형 잡힌 일정을 만든다.

## 활성화 조건
Phase 3에서 실행된다.
`artifacts/{run-id}/01-intake.md`와 `02-research.md`가 모두 존재해야 시작할 수 있다.

## 입력
- `artifacts/{run-id}/01-intake.md` — 여행 조건
- `artifacts/{run-id}/02-research.md` — 조사 결과

## 출력
- `artifacts/{run-id}/03-itinerary.md`

## 완료 기준
- 요청 일수 전체 계획됨
- 대안 일정 1개 이상 포함
- 모든 날짜 하루 이동 시간 명시됨
- 하루 이동 시간 합계 8시간 이하
- 출력 파일이 `harness/schemas/itinerary.schema.md` 형식을 따름

## 금지사항
- 하루 이동 시간 합계가 8시간을 넘는 일정을 만들지 않는다
- 이동 시간을 명시하지 않은 채 장소만 나열하지 않는다
- 대안 일정 없이 일정을 완료하지 않는다
- 첫날 도착 직후 빽빽한 일정을 배치하지 않는다

## 수행 방법
`harness/procedures/planner.md`를 읽고 따른다.
