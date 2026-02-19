# 브레이크포인트 참조 가이드

## 주요 프레임워크 브레이크포인트 비교

| 크기 | Tailwind | Bootstrap | MUI | Chakra UI |
|------|----------|-----------|-----|-----------|
| XS | - | - | 0px | 0px |
| SM | 640px | 576px | 600px | 480px |
| MD | 768px | 768px | 900px | 768px |
| LG | 1024px | 992px | 1200px | 992px |
| XL | 1280px | 1200px | 1536px | 1280px |
| 2XL | 1536px | 1400px | - | 1536px |

## Tailwind CSS 브레이크포인트

### 기본 설정
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      'sm': '640px',
      'md': '768px',
      'lg': '1024px',
      'xl': '1280px',
      '2xl': '1536px',
    }
  }
}
```

### 커스텀 브레이크포인트
```javascript
module.exports = {
  theme: {
    screens: {
      'mobile': '320px',
      'tablet': '640px',
      'laptop': '1024px',
      'desktop': '1280px',
      'wide': '1536px',

      // max-width 브레이크포인트
      'max-md': { 'max': '767px' },

      // 범위 브레이크포인트
      'tablet-only': { 'min': '640px', 'max': '1023px' },
    }
  }
}
```

### 사용 예시
```html
<!-- 기본 (모바일) -->
<div class="text-sm md:text-base lg:text-lg">
  반응형 텍스트
</div>

<!-- max-width 사용 -->
<div class="max-md:hidden">
  태블릿 이상에서만 표시
</div>
```

## CSS 미디어 쿼리 작성

### Mobile-First (min-width)
```css
/* 기본: 모바일 (0px~) */
.element { font-size: 14px; }

/* 태블릿 (768px~) */
@media (min-width: 768px) {
  .element { font-size: 16px; }
}

/* 데스크톱 (1024px~) */
@media (min-width: 1024px) {
  .element { font-size: 18px; }
}
```

### Desktop-First (max-width)
```css
/* 기본: 데스크톱 */
.element { font-size: 18px; }

/* 태블릿 이하 (~1023px) */
@media (max-width: 1023px) {
  .element { font-size: 16px; }
}

/* 모바일 이하 (~767px) */
@media (max-width: 767px) {
  .element { font-size: 14px; }
}
```

### 범위 쿼리
```css
/* 태블릿만 (768px ~ 1023px) */
@media (min-width: 768px) and (max-width: 1023px) {
  .element { /* 태블릿 전용 스타일 */ }
}

/* 새로운 범위 문법 (Level 4) */
@media (768px <= width < 1024px) {
  .element { /* 태블릿 전용 스타일 */ }
}
```

## 디바이스별 일반적인 해상도

### 모바일
| 디바이스 | 뷰포트 너비 | DPR |
|---------|------------|-----|
| iPhone SE | 375px | 2x |
| iPhone 14 | 390px | 3x |
| iPhone 14 Pro Max | 430px | 3x |
| Galaxy S23 | 360px | 3x |
| Pixel 7 | 412px | 2.625x |

### 태블릿
| 디바이스 | 뷰포트 너비 | DPR |
|---------|------------|-----|
| iPad Mini | 744px | 2x |
| iPad (10th) | 820px | 2x |
| iPad Pro 11" | 834px | 2x |
| iPad Pro 12.9" | 1024px | 2x |
| Galaxy Tab S8 | 800px | 2x |

### 데스크톱
| 해상도 | 뷰포트 너비 (대략) |
|--------|-------------------|
| HD (1366×768) | ~1300px |
| Full HD (1920×1080) | ~1850px |
| QHD (2560×1440) | ~2500px |
| 4K (3840×2160) | ~3800px |

## 콘텐츠 기반 브레이크포인트 설계

### 텍스트 가독성 기준
```css
/* 이상적인 줄 길이: 45-75 문자 */
.prose {
  max-width: 65ch; /* 약 65문자 */
}

/* 텍스트 컨테이너 기반 브레이크포인트 */
@media (min-width: 45ch) { /* 최소 가독성 */ }
@media (min-width: 65ch) { /* 이상적 가독성 */ }
```

### 그리드 아이템 기준
```css
/* 카드 최소 너비 기반 */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}

/* 아이템 개수 기반 브레이크포인트 */
/* 1컬럼: ~559px (560 - 280) */
/* 2컬럼: 560px~ */
/* 3컬럼: 840px~ */
/* 4컬럼: 1120px~ */
```

## CSS 변수를 활용한 브레이크포인트 관리

```css
:root {
  --bp-sm: 640px;
  --bp-md: 768px;
  --bp-lg: 1024px;
  --bp-xl: 1280px;
}

/* JavaScript에서 참조 */
```

```javascript
// CSS 변수 읽기
const breakpoints = {
  sm: parseInt(getComputedStyle(document.documentElement).getPropertyValue('--bp-sm')),
  md: parseInt(getComputedStyle(document.documentElement).getPropertyValue('--bp-md')),
  lg: parseInt(getComputedStyle(document.documentElement).getPropertyValue('--bp-lg')),
  xl: parseInt(getComputedStyle(document.documentElement).getPropertyValue('--bp-xl')),
};

// matchMedia 사용
const isTablet = window.matchMedia(`(min-width: ${breakpoints.md}px)`).matches;
```

## Container Queries 브레이크포인트

```css
.card-wrapper {
  container-type: inline-size;
  container-name: card;
}

/* 컨테이너 크기 기반 */
@container card (min-width: 300px) {
  .card { flex-direction: row; }
}

@container card (min-width: 500px) {
  .card { /* 더 큰 레이아웃 */ }
}
```

## 권장 브레이크포인트 세트

### 미니멀 (3개)
```css
/* 모바일: 0-767px */
/* 태블릿: 768-1023px */
/* 데스크톱: 1024px+ */

@media (min-width: 768px) { /* 태블릿 */ }
@media (min-width: 1024px) { /* 데스크톱 */ }
```

### 표준 (5개)
```css
/* xs: 0-479px (소형 모바일) */
/* sm: 480-767px (대형 모바일) */
/* md: 768-1023px (태블릿) */
/* lg: 1024-1279px (노트북) */
/* xl: 1280px+ (데스크톱) */

@media (min-width: 480px) { /* sm */ }
@media (min-width: 768px) { /* md */ }
@media (min-width: 1024px) { /* lg */ }
@media (min-width: 1280px) { /* xl */ }
```

### 확장 (7개) - 대규모 프로젝트
```css
@media (min-width: 480px) { /* mobile-lg */ }
@media (min-width: 640px) { /* tablet-sm */ }
@media (min-width: 768px) { /* tablet */ }
@media (min-width: 1024px) { /* laptop */ }
@media (min-width: 1280px) { /* desktop */ }
@media (min-width: 1536px) { /* desktop-lg */ }
@media (min-width: 1920px) { /* wide */ }
```
