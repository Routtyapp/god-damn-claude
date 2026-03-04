# Gradient Text

## 1. Flowing Gradient Text — CSS animation

```css
/* 흐르는 그라디언트 — background-size 200% + animation */
.gradient-text {
  background: linear-gradient(
    90deg,
    #6366f1 0%,
    #a855f7 25%,
    #ec4899 50%,
    #a855f7 75%,
    #6366f1 100%
  );
  background-size: 200% auto;
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;   /* fallback */

  animation: gradient-flow 3s linear infinite;
}

@keyframes gradient-flow {
  from { background-position: 0% center; }
  to   { background-position: 200% center; }
}

@media (prefers-reduced-motion: reduce) {
  .gradient-text { animation: none; background-position: 0% center; }
}
```

```tsx
// GradientText.tsx
interface GradientTextProps {
  children: React.ReactNode;
  colors?: string[];      // 그라디언트 색상 배열
  angle?: number;         // 기본 90
  speed?: number;         // animation duration 초, 기본 3
  className?: string;
  as?: React.ElementType;
}

export function GradientText({
  children,
  colors = ['#6366f1', '#a855f7', '#ec4899'],
  angle = 90,
  speed = 3,
  className,
  as: Tag = 'span',
}: GradientTextProps) {
  // 루프를 위해 색상 배열 앞뒤 연결
  const stops = [...colors, ...colors];
  const gradient = `linear-gradient(${angle}deg, ${stops.join(', ')})`;

  return (
    <Tag
      className={className}
      style={{
        background: gradient,
        backgroundSize: '200% auto',
        WebkitBackgroundClip: 'text',
        backgroundClip: 'text',
        WebkitTextFillColor: 'transparent',
        color: 'transparent',
        animation: `gradient-flow ${speed}s linear infinite`,
      }}
    >
      {children}
    </Tag>
  );
}
```

---

## 2. Shine Text — 빛 반사 sweep

```tsx
// ShineText.tsx
import styles from './ShineText.module.css';

interface ShineTextProps {
  children: React.ReactNode;
  trigger?: 'hover' | 'auto';
  speed?: number;         // sweep duration 초, 기본 1.5
  className?: string;
}

export function ShineText({ children, trigger = 'hover', speed = 1.5, className }: ShineTextProps) {
  return (
    <span
      className={[
        styles.wrap,
        trigger === 'auto' && styles.auto,
        className,
      ].filter(Boolean).join(' ')}
      style={{ '--speed': `${speed}s` } as React.CSSProperties}
    >
      {children}
      <span className={styles.shine} aria-hidden="true" />
    </span>
  );
}
```

```css
/* ShineText.module.css */
.wrap {
  position: relative;
  display: inline-block;
  overflow: hidden;
}

.shine {
  position: absolute;
  top: 0;
  left: -100%;
  width: 60%;
  height: 100%;
  background: linear-gradient(
    105deg,
    transparent 20%,
    rgba(255,255,255,0.4) 50%,
    transparent 80%
  );
  transform: skewX(-15deg);
  pointer-events: none;
}

/* hover 트리거 */
.wrap:hover .shine {
  animation: shine-sweep var(--speed, 1.5s) ease forwards;
}

/* auto 트리거 (반복) */
.auto .shine {
  animation: shine-sweep var(--speed, 1.5s) ease infinite;
  animation-delay: 1s;
}

@keyframes shine-sweep {
  from { left: -100%; }
  to   { left: 150%; }
}

@media (prefers-reduced-motion: reduce) {
  .shine { display: none; }
}
```

---

## 3. CSS @property Animated Gradient

CSS Houdini를 활용한 색상 값 자체 애니메이션.

```css
/* @property — 색상 커스텀 속성 애니메이션 */
@property --color-a {
  syntax: '<color>';
  initial-value: #6366f1;
  inherits: false;
}

@property --color-b {
  syntax: '<color>';
  initial-value: #ec4899;
  inherits: false;
}

.animated-gradient-text {
  background: linear-gradient(90deg, var(--color-a), var(--color-b));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: color-morph 3s ease-in-out infinite alternate;
}

@keyframes color-morph {
  from { --color-a: #6366f1; --color-b: #ec4899; }
  to   { --color-a: #22d3ee; --color-b: #a855f7; }
}
```

> **브라우저 지원**: Chrome 85+, Safari 15.4+. Firefox 미지원.
> 폴백: `@supports`로 감싸고 미지원 시 정적 그라디언트 표시.

---

## 주의사항

| 항목 | 기준 |
|------|------|
| `background-clip: text` | 반드시 `color: transparent` 또는 `-webkit-text-fill-color: transparent` 함께 설정 |
| 폴백 | `-webkit-text-fill-color` 미지원 브라우저를 위해 `color` 속성도 설정 |
| 인쇄 | 인쇄 시 배경이 사라지면 텍스트도 사라짐 — `@media print { color: inherit; }` 추가 |
| 가독성 | 대비가 낮은 그라디언트는 WCAG 실패 — 단색 fallback 고려 |
