# 타이포그래피 가이드

## 폰트 스택

### 시스템 폰트 스택 (권장)
```css
:root {
  /* Sans-serif (본문용) */
  --font-sans: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
               'Helvetica Neue', Arial, 'Noto Sans', sans-serif,
               'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';

  /* Monospace (코드용) */
  --font-mono: ui-monospace, SFMono-Regular, 'SF Mono', Menlo, Monaco,
               Consolas, 'Liberation Mono', 'Courier New', monospace;

  /* Serif (선택적) */
  --font-serif: ui-serif, Georgia, Cambria, 'Times New Roman', Times, serif;
}
```

### 한글 폰트 스택
```css
:root {
  /* 한글 + 영문 조합 */
  --font-korean: 'Pretendard Variable', Pretendard, -apple-system,
                 BlinkMacSystemFont, system-ui, Roboto, 'Helvetica Neue',
                 'Segoe UI', 'Apple SD Gothic Neo', 'Noto Sans KR',
                 'Malgun Gothic', 'Apple Color Emoji', 'Segoe UI Emoji',
                 'Segoe UI Symbol', sans-serif;

  /* 고딕 계열 */
  --font-gothic: 'Noto Sans KR', 'Apple SD Gothic Neo', 'Malgun Gothic', sans-serif;

  /* 명조 계열 */
  --font-myungjo: 'Noto Serif KR', 'Apple Myungjo', 'Batang', serif;
}
```

### 웹폰트 로딩 최적화
```html
<!-- Preconnect -->
<link rel="preconnect" href="https://cdn.jsdelivr.net" crossorigin>

<!-- Font-display: swap -->
<style>
  @font-face {
    font-family: 'Pretendard';
    src: url('path/to/pretendard.woff2') format('woff2');
    font-weight: 100 900;
    font-display: swap;
  }
</style>
```

## 폰트 크기 스케일

### 고정 스케일
```css
:root {
  --text-xs: 0.75rem;     /* 12px */
  --text-sm: 0.875rem;    /* 14px */
  --text-base: 1rem;      /* 16px */
  --text-lg: 1.125rem;    /* 18px */
  --text-xl: 1.25rem;     /* 20px */
  --text-2xl: 1.5rem;     /* 24px */
  --text-3xl: 1.875rem;   /* 30px */
  --text-4xl: 2.25rem;    /* 36px */
  --text-5xl: 3rem;       /* 48px */
  --text-6xl: 3.75rem;    /* 60px */
  --text-7xl: 4.5rem;     /* 72px */
  --text-8xl: 6rem;       /* 96px */
  --text-9xl: 8rem;       /* 128px */
}
```

### Fluid 타이포그래피 (반응형)
```css
:root {
  /* clamp(최소값, 선호값, 최대값) */
  --text-fluid-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
  --text-fluid-sm: clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
  --text-fluid-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
  --text-fluid-lg: clamp(1.125rem, 1rem + 0.625vw, 1.25rem);
  --text-fluid-xl: clamp(1.25rem, 1rem + 1.25vw, 1.75rem);
  --text-fluid-2xl: clamp(1.5rem, 1rem + 2.5vw, 2.5rem);
  --text-fluid-3xl: clamp(1.875rem, 1rem + 4.375vw, 3.75rem);
  --text-fluid-4xl: clamp(2.25rem, 1rem + 6.25vw, 5rem);
}
```

### Type Scale 비율
```css
/* 일반적인 Type Scale 비율 */
/* Minor Second: 1.067 */
/* Major Second: 1.125 */
/* Minor Third: 1.200 */
/* Major Third: 1.250 (권장) */
/* Perfect Fourth: 1.333 */
/* Golden Ratio: 1.618 */

:root {
  --scale-ratio: 1.25; /* Major Third */
  --text-base-size: 1rem;

  --text-sm: calc(var(--text-base-size) / var(--scale-ratio));
  --text-base: var(--text-base-size);
  --text-lg: calc(var(--text-base-size) * var(--scale-ratio));
  --text-xl: calc(var(--text-base-size) * var(--scale-ratio) * var(--scale-ratio));
  --text-2xl: calc(var(--text-base-size) * var(--scale-ratio) * var(--scale-ratio) * var(--scale-ratio));
}
```

## 폰트 두께

```css
:root {
  --font-thin: 100;
  --font-extralight: 200;
  --font-light: 300;
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
  --font-extrabold: 800;
  --font-black: 900;
}
```

## 줄 높이

```css
:root {
  --leading-none: 1;
  --leading-tight: 1.25;
  --leading-snug: 1.375;
  --leading-normal: 1.5;
  --leading-relaxed: 1.625;
  --leading-loose: 2;

  /* 제목용 */
  --leading-heading: 1.2;

  /* 본문용 */
  --leading-body: 1.6;
}
```

## 자간 (Letter Spacing)

```css
:root {
  --tracking-tighter: -0.05em;
  --tracking-tight: -0.025em;
  --tracking-normal: 0em;
  --tracking-wide: 0.025em;
  --tracking-wider: 0.05em;
  --tracking-widest: 0.1em;
}
```

## 텍스트 스타일 프리셋

```css
/* 제목 스타일 */
.heading-1 {
  font-size: var(--text-4xl);
  font-weight: var(--font-bold);
  line-height: var(--leading-tight);
  letter-spacing: var(--tracking-tight);
}

.heading-2 {
  font-size: var(--text-3xl);
  font-weight: var(--font-semibold);
  line-height: var(--leading-tight);
  letter-spacing: var(--tracking-tight);
}

.heading-3 {
  font-size: var(--text-2xl);
  font-weight: var(--font-semibold);
  line-height: var(--leading-snug);
}

.heading-4 {
  font-size: var(--text-xl);
  font-weight: var(--font-medium);
  line-height: var(--leading-snug);
}

/* 본문 스타일 */
.body-large {
  font-size: var(--text-lg);
  line-height: var(--leading-relaxed);
}

.body-base {
  font-size: var(--text-base);
  line-height: var(--leading-normal);
}

.body-small {
  font-size: var(--text-sm);
  line-height: var(--leading-normal);
}

/* 캡션 스타일 */
.caption {
  font-size: var(--text-xs);
  line-height: var(--leading-normal);
  letter-spacing: var(--tracking-wide);
}

/* 라벨 스타일 */
.label {
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  line-height: var(--leading-none);
}

/* 코드 스타일 */
.code {
  font-family: var(--font-mono);
  font-size: 0.875em;
}
```

## 가독성 최적화

### 최적 줄 길이
```css
.prose {
  /* 이상적인 줄 길이: 45-75자 */
  max-width: 65ch;
}

.prose-narrow {
  max-width: 45ch;
}

.prose-wide {
  max-width: 75ch;
}
```

### 단락 스타일
```css
.prose p {
  margin-bottom: 1.5em;
}

.prose p + p {
  text-indent: 1em; /* 선택적: 첫 줄 들여쓰기 */
}
```

### 제목 간격
```css
.prose h1,
.prose h2,
.prose h3,
.prose h4 {
  margin-top: 2em;
  margin-bottom: 0.5em;
}

/* 첫 번째 제목은 상단 여백 없음 */
.prose > :first-child {
  margin-top: 0;
}
```

## 반응형 타이포그래피

```css
/* Mobile-first 접근 */
html {
  font-size: 16px;
}

@media (min-width: 640px) {
  html {
    font-size: 17px;
  }
}

@media (min-width: 1024px) {
  html {
    font-size: 18px;
  }
}

/* 또는 Fluid 방식 */
html {
  font-size: clamp(16px, 14px + 0.5vw, 20px);
}
```

## 접근성 고려사항

```css
/* 최소 폰트 크기 (모바일에서도 14px 이상 권장) */
body {
  font-size: max(1rem, 14px);
}

/* 충분한 줄 높이 */
p {
  line-height: 1.5;
}

/* 충분한 대비 */
body {
  color: #1f2937; /* 배경이 흰색일 때 4.5:1 이상 대비 */
}

/* 사용자 설정 존중 */
@media (prefers-reduced-motion: reduce) {
  * {
    transition: none;
  }
}

/* 큰 텍스트 선호 */
@media (prefers-contrast: more) {
  body {
    font-weight: 500;
  }
}
```

## 다국어 타이포그래피

```css
/* 언어별 폰트 조정 */
:lang(ko) {
  word-break: keep-all; /* 한글 단어 단위 줄바꿈 */
  overflow-wrap: break-word;
}

:lang(ja) {
  line-height: 1.8; /* 일본어는 조금 더 넓은 줄 높이 */
}

:lang(zh) {
  line-height: 1.8;
}

/* 숫자/영문 폰트 별도 지정 */
.tabular-nums {
  font-variant-numeric: tabular-nums;
  font-feature-settings: 'tnum';
}
```
