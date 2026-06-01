# HTML 리포트 구조 (Report Output Schema)

`artifacts/{run-id}/05-report.html`이 따라야 할 구조.

---

## 필수 4개 섹션

1. **여행 개요 카드** — 목적지, 기간, 인원, 예산 상태를 한눈에
2. **날짜별 일정** — 각 날짜를 카드 또는 탭으로 구분
3. **예산 테이블** — 카테고리별 금액, 총액, 예산 대비 상태
4. **준비 체크리스트** — 출발 전/현지/귀국 후 할 일

## 스타일 규칙

- TailwindCSS CDN 하나만 사용: `<script src="https://cdn.tailwindcss.com"></script>`
- 예산 초과 항목: `text-red-600`
- 예산 여유 항목: `text-green-600`
- 모바일 폭(320px) 대응 필수

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
