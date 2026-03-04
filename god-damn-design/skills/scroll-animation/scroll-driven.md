# CSS Scroll-driven Animation

JS 없이 CSS `animation-timeline`으로 스크롤 연동 애니메이션. Chrome 115+, Safari 18+.

## 브라우저 지원 확인 + 폴백

```css
/* 지원 여부 체크 */
@supports (animation-timeline: scroll()) {
  /* scroll-driven 적용 */
}

/* 지원 안 할 때 기본값 유지 — 애니메이션 없이 정상 표시 */
```

---

## 1. Scroll Progress — 페이지 전체 스크롤 진행도

```css
/* 스크롤 진행바 */
.scroll-bar {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 3px;
  background: linear-gradient(90deg, #6366f1, #a855f7, #ec4899);
  transform-origin: left;
  transform: scaleX(0);
  z-index: 9999;

  @supports (animation-timeline: scroll()) {
    animation: progress-grow linear;
    animation-timeline: scroll(root block);
    animation-fill-mode: both;
  }
}

@keyframes progress-grow {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}
```

---

## 2. View Timeline — 요소가 뷰포트를 통과하는 구간에 연동

```css
/*
  animation-timeline: view()
  entry: 요소 하단이 뷰포트 하단에 진입
  exit:  요소 상단이 뷰포트 상단을 벗어남
*/

.fade-in-section {
  opacity: 0;
  transform: translateY(40px);

  @supports (animation-timeline: scroll()) {
    animation: section-reveal linear both;
    animation-timeline: view();
    /* 뷰포트 진입 20%~40% 구간에서 실행 */
    animation-range: entry 0% entry 30%;
  }
}

@keyframes section-reveal {
  from { opacity: 0; transform: translateY(40px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

---

## 3. Stagger — CSS `animation-delay` + `@counter-style`

```css
/*
  sibling-index()는 아직 실험적(2024). 현실적 대안: CSS 변수.
  JS로 --i 변수를 설정하거나, :nth-child() 셀렉터로 직접 지정.
*/

.stagger-list > * {
  opacity: 0;
  transform: translateX(-20px);

  @supports (animation-timeline: scroll()) {
    animation: stagger-item linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 40%;
    animation-delay: calc(var(--i, 0) * 60ms);
  }
}

@keyframes stagger-item {
  from { opacity: 0; transform: translateX(-20px); }
  to   { opacity: 1; transform: translateX(0); }
}
```

```tsx
// React — --i 변수 주입
function StaggerList({ items }: { items: string[] }) {
  return (
    <ul className="stagger-list">
      {items.map((item, i) => (
        <li key={i} style={{ '--i': i } as React.CSSProperties}>
          {item}
        </li>
      ))}
    </ul>
  );
}
```

---

## 4. Parallax — CSS만으로

```css
.parallax-img {
  @supports (animation-timeline: scroll()) {
    animation: parallax-move linear both;
    animation-timeline: view();
    animation-range: entry 0% exit 100%;
  }
}

@keyframes parallax-move {
  from { transform: translateY(-15%); }
  to   { transform: translateY(15%); }
}
```

---

## 5. 텍스트 컬러 하이라이트

스크롤하면서 텍스트가 순차적으로 밝아지는 효과.

```css
.highlight-text {
  background: linear-gradient(
    to right,
    #e4e4f0 var(--pct, 0%),
    #3f3f46 var(--pct, 0%)
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;

  @supports (animation-timeline: scroll()) {
    animation: text-highlight linear both;
    animation-timeline: view();
    animation-range: entry 20% exit 20%;
  }
}

@keyframes text-highlight {
  from { --pct: 0%; }
  to   { --pct: 100%; }
}

/* CSS Houdini @property 필요 */
@property --pct {
  syntax: '<percentage>';
  initial-value: 0%;
  inherits: false;
}
```

---

## 주의사항

| 항목 | 기준 |
|------|------|
| 브라우저 지원 | `@supports` 필수 — 미지원 시 정상 표시 보장 |
| `animation-fill-mode` | `both` — 시작/끝 상태 고정 |
| `prefers-reduced-motion` | `@media` 블록으로 `animation: none` 오버라이드 |
| 성능 | `transform`, `opacity` 만 사용 — compositor에서 처리 |

```css
@media (prefers-reduced-motion: reduce) {
  [style*="animation-timeline"] {
    animation: none;
    opacity: 1;
    transform: none;
  }
}
```
