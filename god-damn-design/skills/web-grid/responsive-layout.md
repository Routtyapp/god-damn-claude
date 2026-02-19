# Responsive Layout Skill

반응형 레이아웃 설계를 위한 가이드입니다.

## 사용 시점

- 반응형 웹사이트/앱을 설계할 때
- 레이아웃 시스템을 구축할 때
- Mobile-first 또는 Desktop-first 전략을 선택할 때

## 브레이크포인트 전략

### Tailwind CSS 기본값 (권장)
```css
/* Mobile first */
sm: 640px   /* 소형 태블릿 */
md: 768px   /* 태블릿 */
lg: 1024px  /* 노트북 */
xl: 1280px  /* 데스크톱 */
2xl: 1536px /* 대형 모니터 */
```

### Bootstrap 기본값
```css
sm: 576px
md: 768px
lg: 992px
xl: 1200px
xxl: 1400px
```

### 콘텐츠 기반 브레이크포인트 (권장 접근법)
```css
/* 디바이스가 아닌 콘텐츠 기준 */
@media (min-width: 30em) { /* ~480px - 텍스트 라인 길이 */}
@media (min-width: 48em) { /* ~768px - 2컬럼 레이아웃 */}
@media (min-width: 64em) { /* ~1024px - 3컬럼 레이아웃 */}
```

## Mobile-First vs Desktop-First

### Mobile-First (권장)
```css
/* 기본: 모바일 스타일 */
.container {
  padding: 1rem;
}

/* 점진적 확장 */
@media (min-width: 768px) {
  .container {
    padding: 2rem;
    max-width: 720px;
  }
}

@media (min-width: 1024px) {
  .container {
    padding: 3rem;
    max-width: 960px;
  }
}
```

**장점:**
- 성능 최적화 (모바일에 불필요한 CSS 로드 방지)
- 핵심 콘텐츠 우선
- 점진적 향상

### Desktop-First
```css
/* 기본: 데스크톱 스타일 */
.container {
  max-width: 1200px;
  padding: 3rem;
}

/* 축소 */
@media (max-width: 1023px) {
  .container {
    max-width: 720px;
    padding: 2rem;
  }
}

@media (max-width: 767px) {
  .container {
    padding: 1rem;
  }
}
```

**사용 시점:**
- 기존 데스크톱 사이트 반응형 전환
- 복잡한 데스크톱 UI가 핵심인 경우

## Flexbox 레이아웃 패턴

### 기본 네비게이션
```css
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
}

@media (max-width: 768px) {
  .nav {
    flex-direction: column;
  }
}
```

### 카드 그리드 (Flexbox)
```css
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 1.5rem;
}

.card {
  flex: 1 1 300px; /* 최소 300px, 균등 확장 */
  max-width: 100%;
}
```

### 사이드바 레이아웃
```css
.layout {
  display: flex;
  flex-wrap: wrap;
}

.sidebar {
  flex: 0 0 250px;
}

.main {
  flex: 1 1 0%;
  min-width: 0; /* 오버플로우 방지 */
}

@media (max-width: 768px) {
  .sidebar {
    flex: 1 1 100%;
    order: 2;
  }
  .main {
    order: 1;
  }
}
```

## CSS Grid 레이아웃 패턴

### 자동 반응형 그리드
```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}
```

### auto-fit vs auto-fill
```css
/* auto-fit: 아이템이 적으면 확장 */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));

/* auto-fill: 빈 트랙 유지 */
grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
```

### Holy Grail 레이아웃
```css
.layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "nav    main   aside"
    "footer footer footer";
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

.header { grid-area: header; }
.nav { grid-area: nav; }
.main { grid-area: main; }
.aside { grid-area: aside; }
.footer { grid-area: footer; }

@media (max-width: 768px) {
  .layout {
    grid-template-areas:
      "header"
      "main"
      "nav"
      "aside"
      "footer";
    grid-template-columns: 1fr;
  }
}
```

### 대시보드 그리드
```css
.dashboard {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(150px, auto);
  gap: 1rem;
}

.widget-large {
  grid-column: span 2;
  grid-row: span 2;
}

.widget-wide {
  grid-column: span 2;
}

@media (max-width: 1024px) {
  .dashboard {
    grid-template-columns: repeat(2, 1fr);
  }
  .widget-large {
    grid-column: span 2;
  }
}

@media (max-width: 640px) {
  .dashboard {
    grid-template-columns: 1fr;
  }
  .widget-large,
  .widget-wide {
    grid-column: span 1;
  }
}
```

## Container Queries

```css
/* 컨테이너 정의 */
.card-container {
  container-type: inline-size;
  container-name: card;
}

/* 컨테이너 쿼리 */
@container card (min-width: 400px) {
  .card {
    display: flex;
    flex-direction: row;
  }

  .card-image {
    width: 40%;
  }
}

@container card (min-width: 600px) {
  .card-title {
    font-size: 1.5rem;
  }
}
```

### 컨테이너 쿼리 유닛
```css
.card-title {
  font-size: clamp(1rem, 5cqi, 2rem); /* cqi: container query inline */
}
```

## 반응형 타이포그래피

### Fluid Typography (clamp)
```css
:root {
  /* 최소 16px, 뷰포트의 2%, 최대 24px */
  --fluid-body: clamp(1rem, 0.5rem + 1vw, 1.5rem);

  /* 제목 */
  --fluid-h1: clamp(2rem, 1rem + 3vw, 4rem);
  --fluid-h2: clamp(1.5rem, 0.75rem + 2vw, 3rem);
}

body {
  font-size: var(--fluid-body);
}

h1 {
  font-size: var(--fluid-h1);
}
```

### 반응형 스페이싱
```css
:root {
  --space-xs: clamp(0.25rem, 0.5vw, 0.5rem);
  --space-sm: clamp(0.5rem, 1vw, 1rem);
  --space-md: clamp(1rem, 2vw, 2rem);
  --space-lg: clamp(2rem, 4vw, 4rem);
  --space-xl: clamp(4rem, 8vw, 8rem);
}
```

## 반응형 이미지

```css
img {
  max-width: 100%;
  height: auto;
}

/* aspect-ratio 유지 */
.video-container {
  aspect-ratio: 16 / 9;
  width: 100%;
}

/* object-fit으로 이미지 조절 */
.cover-image {
  width: 100%;
  height: 300px;
  object-fit: cover;
  object-position: center;
}
```

### srcset과 sizes
```html
<img
  src="image-800.jpg"
  srcset="
    image-400.jpg 400w,
    image-800.jpg 800w,
    image-1200.jpg 1200w
  "
  sizes="
    (max-width: 640px) 100vw,
    (max-width: 1024px) 50vw,
    33vw
  "
  alt="반응형 이미지"
/>
```

## 접근성 고려사항

```css
/* 애니메이션 선호도 존중 */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* 고대비 모드 */
@media (prefers-contrast: high) {
  :root {
    --text-color: #000;
    --bg-color: #fff;
  }
}

/* 터치 디바이스 */
@media (hover: none) and (pointer: coarse) {
  .button {
    min-height: 44px;
    min-width: 44px;
  }
}
```

상세한 브레이크포인트 참조는 [breakpoints.md](breakpoints.md)를 확인하세요.
