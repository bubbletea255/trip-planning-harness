# Report Composer Contract

## 목적
일정표와 예산 분석 결과를 통합해 브라우저에서 바로 열 수 있는 HTML 리포트를 생성한다.
준비 체크리스트도 함께 포함한다.

## 활성화 조건
Phase 5에서 실행된다.
`artifacts/{run-id}/03-itinerary.md`와 `04-budget.md`가 모두 존재해야 시작할 수 있다.

## 입력
- `artifacts/{run-id}/01-intake.md` — 여행 개요
- `artifacts/{run-id}/03-itinerary.md` — 날짜별 일정
- `artifacts/{run-id}/04-budget.md` — 예산 분석 결과

## 출력
- `artifacts/{run-id}/05-report.html`

## 완료 기준
- 4개 섹션(여행 개요, 날짜별 일정, 예산 테이블, 준비 체크리스트) 모두 포함
- 브라우저에서 열 수 있는 단일 HTML 파일
- 출력 파일이 `harness/schemas/report.schema.md` 형식을 따름

## 금지사항
- 외부 이미지 URL을 삽입하지 않는다 — 로딩 실패 시 레이아웃이 깨진다
- Chart.js나 다른 JS 라이브러리를 추가하지 않는다 — TailwindCSS CDN 하나만 사용한다
- 인터넷 연결 없이 열었을 때 레이아웃이 완전히 깨지는 구조를 만들지 않는다
- 4개 섹션 중 하나라도 빠뜨리지 않는다

## 수행 방법
`harness/procedures/report-composer.md`를 읽고 따른다.
