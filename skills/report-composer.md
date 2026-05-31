# Report Composer 실행 지침

## 작업 절차

1. `artifacts/{run-id}/01-intake.md`, `03-itinerary.md`, `04-budget.md`를 모두 읽는다.
2. HTML 구조를 설계한다. 아래 4개 섹션을 반드시 포함한다.
   - **여행 개요 카드**: 목적지, 기간, 인원, 예산 상태를 한눈에
   - **날짜별 일정**: 각 날짜를 카드 또는 탭으로 구분
   - **예산 테이블**: 카테고리별 금액, 총액, 예산 대비 상태
   - **준비 체크리스트**: 출발 전/현지/귀국 후 할 일
3. 스타일은 TailwindCSS CDN만 사용한다. (`<script src="https://cdn.tailwindcss.com"></script>`)
4. 예산 초과 항목은 빨간색(`text-red-600`), 여유 항목은 초록색(`text-green-600`)으로 표시한다.
5. 모바일 폭(320px)에서 텍스트 겹침, 표 넘침이 없는지 검토한다.
6. `artifacts/{run-id}/05-report.html`로 저장한다.

---

## HTML 기본 구조

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[목적지] 여행 계획</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-stone-50 text-slate-900">
  <main class="mx-auto max-w-4xl px-4 py-8">
    <!-- 1. 여행 개요 카드 -->
    <!-- 2. 날짜별 일정 -->
    <!-- 3. 예산 테이블 -->
    <!-- 4. 준비 체크리스트 -->
  </main>
</body>
</html>
```

---

## 준비 체크리스트 기준 항목

- **출발 전**: 여권 유효기간, 비자, 항공권 예약 확인, 숙소 예약 확인, 여행자보험, 환전, 필수 앱 설치
- **예약 필수**: intake에서 언급된 특정 장소·식당 예약 (researcher가 표시한 항목 우선)
- **현지**: 숙소 체크인 시간 확인, 예약 식당 리마인더, 귀국 항공편 확인
- **귀국 후**: 지출 정산

---

## 하지 말아야 할 것

- 외부 이미지 URL을 삽입하지 않는다. (로딩 실패 시 레이아웃이 깨진다)
- Chart.js나 다른 JS 라이브러리를 추가하지 않는다. TailwindCSS CDN 하나만 사용한다.
- 인터넷 연결 없이 열었을 때 레이아웃이 완전히 깨지는 구조를 만들지 않는다.
- 4개 섹션 중 하나라도 빠뜨리지 않는다.
