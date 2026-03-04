# Highlight Reveal

스크롤 진입 시 텍스트 배경이 왼쪽에서 오른쪽으로 닦이며 나타나는 효과.
`background-size: 0% → 100%` 전환이 핵심. GSAP 없이 IntersectionObserver로 구현.

---

## 핵심 원리

```css
.highlight {
  background-repeat: no-repeat;
  background-size: 0% 100%;   /* 초기: 너비 0 */
  transition: background-size 1s cubic-bezier(0.25, 1, 0.5, 1);
}
.highlight.active {
  background-size: 100% 100%; /* 활성: 왼쪽 → 오른쪽 wipe-in */
}
```

---

## 3가지 스타일

```css
/* 1. Background — 전체 배경 채우기 */
[data-highlight="background"] .highlight {
  background-image: linear-gradient(var(--highlight-color), var(--highlight-color));
}

/* 2. Half — 형광펜 (하단 50%) */
[data-highlight="half"] .highlight {
  background-image: linear-gradient(
    transparent 50%,
    var(--highlight-color) 50%
  );
}

/* 3. Underline — 얇은 밑줄 */
[data-highlight="underline"] .highlight {
  background-image: linear-gradient(
    transparent calc(100% - 0.12em),
    var(--highlight-color) 0.12em
  );
}
```

---

## TextHighlight 컴포넌트

```tsx
// TextHighlight.tsx
import { useEffect, useRef } from 'react';
import styles from './TextHighlight.module.css';

type HighlightStyle = 'background' | 'half' | 'underline';

interface TextHighlightProps {
  children: React.ReactNode;
  style?: HighlightStyle;
  color?: string;
  duration?: number;       // ms, 기본 1000
  delay?: number;          // ms, 기본 0
  threshold?: number;      // IntersectionObserver, 기본 0.5
  className?: string;
}

export function TextHighlight({
  children,
  style = 'background',
  color,
  duration = 1000,
  delay = 0,
  threshold = 0.5,
  className,
}: TextHighlightProps) {
  const ref = useRef<HTMLMarkElement>(null);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) {
      el.classList.add(styles.active);
      return;
    }

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setTimeout(() => el.classList.add(styles.active), delay);
          observer.unobserve(el);
        }
      },
      { threshold }
    );

    observer.observe(el);
    return () => observer.disconnect();
  }, [delay, threshold]);

  return (
    <mark
      ref={ref}
      className={[styles.highlight, styles[style], className].filter(Boolean).join(' ')}
      style={{
        '--highlight-color': color,
        '--duration': `${duration}ms`,
      } as React.CSSProperties}
    >
      {/* 스크린리더용 시각 숨김 텍스트 */}
      <span className={styles.srOnly}>[강조 시작]</span>
      {children}
      <span className={styles.srOnly}>[강조 끝]</span>
    </mark>
  );
}
```

```css
/* TextHighlight.module.css */
.highlight {
  all: unset;
  background-repeat: no-repeat;
  background-size: 0% 100%;
  transition:
    background-size var(--duration, 1000ms) cubic-bezier(0.25, 1, 0.5, 1),
    color 250ms ease;
}

.active {
  background-size: 100% 100%;
}

/* 스타일별 배경 이미지 */
.background {
  --highlight-color: hsl(60, 90%, 50%);
  background-image: linear-gradient(var(--highlight-color), var(--highlight-color));
}
.background.active { color: #000; }

.half {
  --highlight-color: hsl(60, 90%, 50%);
  background-image: linear-gradient(
    transparent 50%,
    var(--highlight-color) 50%
  );
}

.underline {
  --highlight-color: currentColor;
  background-image: linear-gradient(
    transparent calc(100% - 0.12em),
    var(--highlight-color) 0.12em
  );
}

/* 접근성 — 숨김 텍스트 */
.srOnly {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}

@media (prefers-reduced-motion: reduce) {
  .highlight { transition: none; }
}
```

---

## 다크모드 대응

```css
/* CSS 변수로 테마 분기 */
:root {
  --highlight-bg: hsl(60, 90%, 50%);   /* 노란색 */
  --highlight-text: #000;
}

[data-theme="dark"] {
  --highlight-bg: hsl(238, 70%, 40%);  /* 인디고 */
  --highlight-text: #fff;
}

.highlight.background {
  --highlight-color: var(--highlight-bg);
}
.highlight.background.active {
  color: var(--highlight-text);
}
```

---

## 복수 하이라이트 — stagger 딜레이

```tsx
// 문단 내 여러 하이라이트를 순차 등장
function HighlightedParagraph() {
  return (
    <p>
      일반 텍스트{' '}
      <TextHighlight delay={0}>첫 번째 강조</TextHighlight>
      {' '}더 많은 텍스트{' '}
      <TextHighlight delay={300}>두 번째 강조</TextHighlight>
      {' '}마지막 텍스트{' '}
      <TextHighlight delay={600}>세 번째 강조</TextHighlight>.
    </p>
  );
}
```

---

## 사용 시나리오

| 시나리오 | 스타일 | color |
|----------|--------|-------|
| 마케팅 랜딩 키워드 강조 | `background` | 브랜드 컬러 |
| 아티클/블로그 중요 문장 | `half` | 노란색/형광 |
| 기술 문서 용어 표시 | `underline` | 텍스트 색상 |
| 다크 테마 UI | `background` | 인디고/퍼플 |

---

## 주의사항

- `<mark>` 태그 사용 — 의미론적으로 "중요한 텍스트" 표시
- `all: unset` 으로 `<mark>` 기본 스타일(노란 배경) 제거
- `threshold: 0.5` — 요소가 50% 이상 보일 때 트리거. 짧은 인라인 텍스트는 `0.3`으로 낮출 것
- 한 문단에 하이라이트 3개 이상이면 `delay` stagger 필수 — 동시 등장 시 시선 분산
