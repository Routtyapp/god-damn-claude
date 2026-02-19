# 디자인 토큰 상세 가이드

## 토큰 계층 구조

### 1. Primitive Tokens (원시 토큰)
가장 기본적인 값. 의미 없는 순수한 값.

```css
--color-blue-500: #3b82f6;
--space-4: 1rem;
--radius-md: 0.375rem;
```

### 2. Semantic Tokens (의미적 토큰)
원시 토큰에 의미를 부여.

```css
--color-primary: var(--color-blue-500);
--color-error: var(--color-red-500);
--space-component-gap: var(--space-4);
```

### 3. Component Tokens (컴포넌트 토큰)
특정 컴포넌트에 적용되는 토큰.

```css
--btn-bg: var(--color-primary);
--card-padding: var(--space-6);
--input-border-color: var(--color-border);
```

## 색상 토큰

### 색상 스케일 정의
```css
:root {
  /* 회색 계열 */
  --color-gray-50: #f9fafb;
  --color-gray-100: #f3f4f6;
  --color-gray-200: #e5e7eb;
  --color-gray-300: #d1d5db;
  --color-gray-400: #9ca3af;
  --color-gray-500: #6b7280;
  --color-gray-600: #4b5563;
  --color-gray-700: #374151;
  --color-gray-800: #1f2937;
  --color-gray-900: #111827;
  --color-gray-950: #030712;

  /* 파랑 계열 */
  --color-blue-50: #eff6ff;
  --color-blue-100: #dbeafe;
  --color-blue-200: #bfdbfe;
  --color-blue-300: #93c5fd;
  --color-blue-400: #60a5fa;
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-blue-700: #1d4ed8;
  --color-blue-800: #1e40af;
  --color-blue-900: #1e3a8a;

  /* 빨강 계열 */
  --color-red-50: #fef2f2;
  --color-red-100: #fee2e2;
  --color-red-500: #ef4444;
  --color-red-600: #dc2626;
  --color-red-700: #b91c1c;

  /* 초록 계열 */
  --color-green-50: #f0fdf4;
  --color-green-100: #dcfce7;
  --color-green-500: #22c55e;
  --color-green-600: #16a34a;
  --color-green-700: #15803d;

  /* 노랑/주황 계열 */
  --color-yellow-50: #fefce8;
  --color-yellow-500: #eab308;
  --color-orange-500: #f97316;
}
```

### 의미적 색상 매핑
```css
:root {
  /* 배경 */
  --color-bg-primary: #ffffff;
  --color-bg-secondary: var(--color-gray-50);
  --color-bg-tertiary: var(--color-gray-100);
  --color-bg-inverse: var(--color-gray-900);

  /* 텍스트 */
  --color-text-primary: var(--color-gray-900);
  --color-text-secondary: var(--color-gray-600);
  --color-text-tertiary: var(--color-gray-400);
  --color-text-inverse: #ffffff;
  --color-text-link: var(--color-blue-600);
  --color-text-link-hover: var(--color-blue-700);

  /* 보더 */
  --color-border-default: var(--color-gray-200);
  --color-border-strong: var(--color-gray-300);
  --color-border-focus: var(--color-blue-500);

  /* 인터랙션 */
  --color-interactive-primary: var(--color-blue-500);
  --color-interactive-primary-hover: var(--color-blue-600);
  --color-interactive-primary-active: var(--color-blue-700);

  /* 상태 */
  --color-status-success: var(--color-green-500);
  --color-status-success-bg: var(--color-green-50);
  --color-status-warning: var(--color-yellow-500);
  --color-status-warning-bg: var(--color-yellow-50);
  --color-status-error: var(--color-red-500);
  --color-status-error-bg: var(--color-red-50);
  --color-status-info: var(--color-blue-500);
  --color-status-info-bg: var(--color-blue-50);
}
```

### 다크모드 색상
```css
.dark {
  --color-bg-primary: var(--color-gray-900);
  --color-bg-secondary: var(--color-gray-800);
  --color-bg-tertiary: var(--color-gray-700);
  --color-bg-inverse: #ffffff;

  --color-text-primary: var(--color-gray-50);
  --color-text-secondary: var(--color-gray-300);
  --color-text-tertiary: var(--color-gray-500);
  --color-text-inverse: var(--color-gray-900);

  --color-border-default: var(--color-gray-700);
  --color-border-strong: var(--color-gray-600);
}
```

## 스페이싱 토큰

### 기본 스케일 (4px 기반)
```css
:root {
  --space-0: 0;
  --space-px: 1px;
  --space-0-5: 0.125rem;  /* 2px */
  --space-1: 0.25rem;     /* 4px */
  --space-1-5: 0.375rem;  /* 6px */
  --space-2: 0.5rem;      /* 8px */
  --space-2-5: 0.625rem;  /* 10px */
  --space-3: 0.75rem;     /* 12px */
  --space-3-5: 0.875rem;  /* 14px */
  --space-4: 1rem;        /* 16px */
  --space-5: 1.25rem;     /* 20px */
  --space-6: 1.5rem;      /* 24px */
  --space-7: 1.75rem;     /* 28px */
  --space-8: 2rem;        /* 32px */
  --space-9: 2.25rem;     /* 36px */
  --space-10: 2.5rem;     /* 40px */
  --space-11: 2.75rem;    /* 44px */
  --space-12: 3rem;       /* 48px */
  --space-14: 3.5rem;     /* 56px */
  --space-16: 4rem;       /* 64px */
  --space-20: 5rem;       /* 80px */
  --space-24: 6rem;       /* 96px */
  --space-28: 7rem;       /* 112px */
  --space-32: 8rem;       /* 128px */
}
```

### 의미적 스페이싱
```css
:root {
  /* 인라인 (좌우) */
  --space-inline-xs: var(--space-1);
  --space-inline-sm: var(--space-2);
  --space-inline-md: var(--space-4);
  --space-inline-lg: var(--space-6);
  --space-inline-xl: var(--space-8);

  /* 블록 (상하) */
  --space-block-xs: var(--space-1);
  --space-block-sm: var(--space-2);
  --space-block-md: var(--space-4);
  --space-block-lg: var(--space-8);
  --space-block-xl: var(--space-12);

  /* 갭 */
  --space-gap-xs: var(--space-1);
  --space-gap-sm: var(--space-2);
  --space-gap-md: var(--space-4);
  --space-gap-lg: var(--space-6);
  --space-gap-xl: var(--space-8);

  /* 섹션 */
  --space-section-sm: var(--space-8);
  --space-section-md: var(--space-16);
  --space-section-lg: var(--space-24);
}
```

## 그림자 토큰

```css
:root {
  /* 기본 그림자 */
  --shadow-xs: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-sm: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
  --shadow-2xl: 0 25px 50px -12px rgb(0 0 0 / 0.25);

  /* 인셋 그림자 */
  --shadow-inner: inset 0 2px 4px 0 rgb(0 0 0 / 0.05);

  /* 포커스 링 */
  --shadow-focus: 0 0 0 3px rgb(59 130 246 / 0.5);

  /* 컬러 그림자 */
  --shadow-primary: 0 4px 14px 0 rgb(59 130 246 / 0.4);
  --shadow-error: 0 4px 14px 0 rgb(239 68 68 / 0.4);
}

/* 다크모드 그림자 */
.dark {
  --shadow-sm: 0 1px 3px 0 rgb(0 0 0 / 0.3);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.4);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.5);
}
```

## 보더 토큰

```css
:root {
  /* 보더 너비 */
  --border-width-0: 0px;
  --border-width-1: 1px;
  --border-width-2: 2px;
  --border-width-4: 4px;
  --border-width-8: 8px;

  /* 보더 반경 */
  --radius-none: 0px;
  --radius-sm: 0.125rem;   /* 2px */
  --radius-md: 0.375rem;   /* 6px */
  --radius-lg: 0.5rem;     /* 8px */
  --radius-xl: 0.75rem;    /* 12px */
  --radius-2xl: 1rem;      /* 16px */
  --radius-3xl: 1.5rem;    /* 24px */
  --radius-full: 9999px;

  /* 의미적 반경 */
  --radius-button: var(--radius-md);
  --radius-input: var(--radius-md);
  --radius-card: var(--radius-lg);
  --radius-modal: var(--radius-xl);
  --radius-badge: var(--radius-full);
}
```

## 애니메이션 토큰

```css
:root {
  /* 지속 시간 */
  --duration-instant: 0ms;
  --duration-fast: 100ms;
  --duration-normal: 200ms;
  --duration-slow: 300ms;
  --duration-slower: 500ms;

  /* 이징 함수 */
  --ease-linear: linear;
  --ease-in: cubic-bezier(0.4, 0, 1, 1);
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55);

  /* 조합 */
  --transition-colors: color var(--duration-normal) var(--ease-in-out),
                       background-color var(--duration-normal) var(--ease-in-out),
                       border-color var(--duration-normal) var(--ease-in-out);
  --transition-opacity: opacity var(--duration-normal) var(--ease-in-out);
  --transition-transform: transform var(--duration-normal) var(--ease-out);
  --transition-all: all var(--duration-normal) var(--ease-in-out);
}
```

## Z-Index 토큰

```css
:root {
  --z-below: -1;
  --z-base: 0;
  --z-raised: 1;
  --z-dropdown: 1000;
  --z-sticky: 1100;
  --z-fixed: 1200;
  --z-modal-backdrop: 1300;
  --z-modal: 1400;
  --z-popover: 1500;
  --z-tooltip: 1600;
  --z-toast: 1700;
}
```

## 토큰 네이밍 컨벤션

### 권장 패턴
```
--{category}-{property}-{variant}-{state}

예시:
--color-bg-primary
--color-text-secondary-hover
--space-inline-md
--btn-bg-primary-active
```

### 카테고리
- `color`: 색상
- `space`: 스페이싱
- `size`: 크기 (width, height)
- `font`: 폰트 관련
- `radius`: 보더 반경
- `shadow`: 그림자
- `z`: Z-Index
- `duration`: 애니메이션 지속 시간
- `ease`: 이징 함수
