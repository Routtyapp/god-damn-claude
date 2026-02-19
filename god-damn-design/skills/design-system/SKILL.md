# Design System Skill

디자인 시스템 구축을 위한 가이드입니다.

## 사용 시점

- 새 프로젝트의 디자인 토큰을 정의할 때
- 일관된 스타일 시스템을 구축할 때
- 테마 시스템 (다크모드 등)을 구현할 때

## 디자인 시스템 핵심 요소

### 1. 디자인 토큰
- 색상 (Colors)
- 타이포그래피 (Typography)
- 스페이싱 (Spacing)
- 그림자 (Shadows)
- 보더 (Borders)
- 애니메이션 (Motion)

### 2. 컴포넌트 라이브러리
- 기본 컴포넌트 (Button, Input, Card 등)
- 복합 컴포넌트 (Form, Modal, Dropdown 등)
- 레이아웃 컴포넌트 (Container, Grid, Stack 등)

### 3. 패턴 및 가이드라인
- 사용 가이드
- 접근성 가이드
- 브랜딩 가이드

## CSS 변수 기반 토큰 시스템

```css
:root {
  /* === 색상 === */
  /* Primitive Colors (원시 색상) */
  --color-blue-50: #eff6ff;
  --color-blue-100: #dbeafe;
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-blue-700: #1d4ed8;

  --color-gray-50: #f9fafb;
  --color-gray-100: #f3f4f6;
  --color-gray-200: #e5e7eb;
  --color-gray-500: #6b7280;
  --color-gray-700: #374151;
  --color-gray-900: #111827;

  /* Semantic Colors (의미적 색상) */
  --color-primary: var(--color-blue-500);
  --color-primary-hover: var(--color-blue-600);
  --color-primary-active: var(--color-blue-700);

  --color-background: #ffffff;
  --color-surface: var(--color-gray-50);
  --color-border: var(--color-gray-200);

  --color-text-primary: var(--color-gray-900);
  --color-text-secondary: var(--color-gray-500);
  --color-text-inverse: #ffffff;

  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
  --color-info: #3b82f6;

  /* === 타이포그래피 === */
  --font-sans: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, 'SF Mono', Menlo, monospace;

  --font-size-xs: 0.75rem;    /* 12px */
  --font-size-sm: 0.875rem;   /* 14px */
  --font-size-base: 1rem;     /* 16px */
  --font-size-lg: 1.125rem;   /* 18px */
  --font-size-xl: 1.25rem;    /* 20px */
  --font-size-2xl: 1.5rem;    /* 24px */
  --font-size-3xl: 1.875rem;  /* 30px */
  --font-size-4xl: 2.25rem;   /* 36px */

  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;

  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;

  /* === 스페이싱 === */
  --space-0: 0;
  --space-1: 0.25rem;   /* 4px */
  --space-2: 0.5rem;    /* 8px */
  --space-3: 0.75rem;   /* 12px */
  --space-4: 1rem;      /* 16px */
  --space-5: 1.25rem;   /* 20px */
  --space-6: 1.5rem;    /* 24px */
  --space-8: 2rem;      /* 32px */
  --space-10: 2.5rem;   /* 40px */
  --space-12: 3rem;     /* 48px */
  --space-16: 4rem;     /* 64px */

  /* === 보더 === */
  --radius-none: 0;
  --radius-sm: 0.125rem;  /* 2px */
  --radius-md: 0.375rem;  /* 6px */
  --radius-lg: 0.5rem;    /* 8px */
  --radius-xl: 0.75rem;   /* 12px */
  --radius-2xl: 1rem;     /* 16px */
  --radius-full: 9999px;

  --border-width: 1px;

  /* === 그림자 === */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);

  /* === 애니메이션 === */
  --duration-fast: 150ms;
  --duration-normal: 300ms;
  --duration-slow: 500ms;

  --easing-default: cubic-bezier(0.4, 0, 0.2, 1);
  --easing-in: cubic-bezier(0.4, 0, 1, 1);
  --easing-out: cubic-bezier(0, 0, 0.2, 1);

  /* === Z-Index === */
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-modal: 300;
  --z-popover: 400;
  --z-tooltip: 500;
}
```

## 다크모드 지원

### CSS 변수 오버라이드
```css
/* 시스템 설정 기반 */
@media (prefers-color-scheme: dark) {
  :root {
    --color-background: #0f172a;
    --color-surface: #1e293b;
    --color-border: #334155;

    --color-text-primary: #f8fafc;
    --color-text-secondary: #94a3b8;

    --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.3);
  }
}

/* 클래스 기반 (수동 전환) */
.dark {
  --color-background: #0f172a;
  --color-surface: #1e293b;
  /* ... */
}
```

### JavaScript 토글
```javascript
function toggleDarkMode() {
  const isDark = document.documentElement.classList.toggle('dark');
  localStorage.setItem('theme', isDark ? 'dark' : 'light');
}

// 초기화
function initTheme() {
  const saved = localStorage.getItem('theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;

  if (saved === 'dark' || (!saved && prefersDark)) {
    document.documentElement.classList.add('dark');
  }
}
```

## Tailwind CSS 설정

```javascript
// tailwind.config.js
module.exports = {
  darkMode: 'class', // 또는 'media'
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          100: '#dbeafe',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
        },
        surface: 'var(--color-surface)',
        border: 'var(--color-border)',
      },
      fontFamily: {
        sans: ['var(--font-sans)'],
        mono: ['var(--font-mono)'],
      },
      fontSize: {
        'fluid-sm': 'clamp(0.875rem, 0.8rem + 0.25vw, 1rem)',
        'fluid-base': 'clamp(1rem, 0.9rem + 0.5vw, 1.25rem)',
        'fluid-lg': 'clamp(1.25rem, 1rem + 1vw, 2rem)',
      },
      spacing: {
        // 커스텀 스페이싱
      },
      borderRadius: {
        DEFAULT: '0.375rem',
      },
      boxShadow: {
        // 커스텀 그림자
      },
    },
  },
  plugins: [],
}
```

## 컴포넌트 토큰 예시

```css
/* Button 토큰 */
:root {
  /* 사이즈 */
  --btn-height-sm: 2rem;
  --btn-height-md: 2.5rem;
  --btn-height-lg: 3rem;

  --btn-padding-sm: 0.75rem;
  --btn-padding-md: 1rem;
  --btn-padding-lg: 1.5rem;

  --btn-font-size-sm: var(--font-size-sm);
  --btn-font-size-md: var(--font-size-base);
  --btn-font-size-lg: var(--font-size-lg);

  /* Primary 버전트 */
  --btn-primary-bg: var(--color-primary);
  --btn-primary-bg-hover: var(--color-primary-hover);
  --btn-primary-text: var(--color-text-inverse);

  /* Secondary 버전트 */
  --btn-secondary-bg: var(--color-surface);
  --btn-secondary-bg-hover: var(--color-gray-100);
  --btn-secondary-text: var(--color-text-primary);
  --btn-secondary-border: var(--color-border);
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-weight: var(--font-weight-medium);
  border-radius: var(--radius-md);
  transition: background-color var(--duration-fast) var(--easing-default);
}

.btn-md {
  height: var(--btn-height-md);
  padding-inline: var(--btn-padding-md);
  font-size: var(--btn-font-size-md);
}

.btn-primary {
  background-color: var(--btn-primary-bg);
  color: var(--btn-primary-text);
}

.btn-primary:hover {
  background-color: var(--btn-primary-bg-hover);
}
```

## 디자인 토큰 구조화 (JSON)

```json
{
  "color": {
    "primitive": {
      "blue": {
        "50": { "value": "#eff6ff" },
        "500": { "value": "#3b82f6" },
        "600": { "value": "#2563eb" }
      }
    },
    "semantic": {
      "primary": { "value": "{color.primitive.blue.500}" },
      "primary-hover": { "value": "{color.primitive.blue.600}" }
    }
  },
  "spacing": {
    "1": { "value": "0.25rem" },
    "2": { "value": "0.5rem" },
    "4": { "value": "1rem" }
  }
}
```

## 참조 문서

- [디자인 토큰 상세](tokens.md)
- [타이포그래피 가이드](typography.md)
