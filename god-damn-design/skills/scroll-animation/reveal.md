# Reveal on Scroll

IntersectionObserver로 뷰포트 진입 시 요소를 등장시키는 패턴.

## 1. useReveal Hook

```tsx
// hooks/useReveal.ts
import { useEffect, useRef } from 'react';

interface RevealOptions {
  threshold?: number;      // 노출 비율 (0–1), 기본 0.15
  rootMargin?: string;     // 뷰포트 여백, 기본 '0px'
  once?: boolean;          // 한 번만 실행 여부, 기본 true
}

export function useReveal<T extends HTMLElement>(options: RevealOptions = {}) {
  const { threshold = 0.15, rootMargin = '0px', once = true } = options;
  const ref = useRef<T>(null);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;

    // prefers-reduced-motion — 즉시 표시
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) {
      el.style.opacity = '1';
      el.style.transform = 'none';
      return;
    }

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          el.classList.add('is-revealed');
          if (once) observer.unobserve(el);
        } else if (!once) {
          el.classList.remove('is-revealed');
        }
      },
      { threshold, rootMargin }
    );

    observer.observe(el);
    return () => observer.disconnect();
  }, [threshold, rootMargin, once]);

  return ref;
}
```

```css
/* globals.css — 전역 1회 선언 */

/* Fade up (기본) */
.reveal-fade {
  opacity: 0;
  transform: translateY(32px);
  transition:
    opacity 600ms cubic-bezier(0.16, 1, 0.3, 1),
    transform 600ms cubic-bezier(0.16, 1, 0.3, 1);
  transition-delay: var(--reveal-delay, 0ms);
}

/* Fade (위치 변화 없음) */
.reveal-opacity {
  opacity: 0;
  transition: opacity 500ms ease;
  transition-delay: var(--reveal-delay, 0ms);
}

/* Slide from left */
.reveal-left {
  opacity: 0;
  transform: translateX(-32px);
  transition:
    opacity 600ms cubic-bezier(0.16, 1, 0.3, 1),
    transform 600ms cubic-bezier(0.16, 1, 0.3, 1);
  transition-delay: var(--reveal-delay, 0ms);
}

/* Slide from right */
.reveal-right {
  opacity: 0;
  transform: translateX(32px);
  transition:
    opacity 600ms cubic-bezier(0.16, 1, 0.3, 1),
    transform 600ms cubic-bezier(0.16, 1, 0.3, 1);
  transition-delay: var(--reveal-delay, 0ms);
}

/* Scale up */
.reveal-scale {
  opacity: 0;
  transform: scale(0.92);
  transition:
    opacity 500ms cubic-bezier(0.16, 1, 0.3, 1),
    transform 500ms cubic-bezier(0.16, 1, 0.3, 1);
  transition-delay: var(--reveal-delay, 0ms);
}

/* Activated state */
.is-revealed {
  opacity: 1 !important;
  transform: none !important;
}

@media (prefers-reduced-motion: reduce) {
  [class*="reveal-"] { transition: none; opacity: 1; transform: none; }
}
```

### 사용 예시

```tsx
// 단일 요소
function HeroSection() {
  const ref = useReveal<HTMLDivElement>();
  return <div ref={ref} className="reveal-fade">Hello</div>;
}

// 지연 없이 복수 요소 (CSS --reveal-delay 활용)
function FeatureGrid() {
  return (
    <ul>
      {features.map((f, i) => (
        <li
          key={f.id}
          className="reveal-fade"
          style={{ '--reveal-delay': `${i * 80}ms` } as React.CSSProperties}
          ref={/* 개별 ref */ undefined}
        >
          {f.name}
        </li>
      ))}
    </ul>
  );
}
```

---

## 2. 그룹 Stagger — useStaggerReveal

여러 자식 요소를 한 번의 Observer로 순차 등장.

```tsx
// hooks/useStaggerReveal.ts
import { useEffect, useRef } from 'react';

interface StaggerRevealOptions {
  stagger?: number;       // 항목 간 딜레이 ms, 기본 60
  threshold?: number;
  childSelector?: string; // 기본 ':scope > *'
}

export function useStaggerReveal<T extends HTMLElement>(options: StaggerRevealOptions = {}) {
  const { stagger = 60, threshold = 0.1, childSelector = ':scope > *' } = options;
  const containerRef = useRef<T>(null);

  useEffect(() => {
    const container = containerRef.current;
    if (!container) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    const children = Array.from(container.querySelectorAll<HTMLElement>(childSelector));

    if (mq.matches) {
      children.forEach(el => { el.style.opacity = '1'; el.style.transform = 'none'; });
      return;
    }

    // 초기 상태 세팅
    children.forEach((el, i) => {
      el.style.setProperty('--reveal-delay', `${i * stagger}ms`);
      el.classList.add('reveal-fade');
    });

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          children.forEach(el => el.classList.add('is-revealed'));
          observer.unobserve(container);
        }
      },
      { threshold }
    );

    observer.observe(container);
    return () => observer.disconnect();
  }, [stagger, threshold, childSelector]);

  return containerRef;
}
```

```tsx
// 사용 예시
function CardGrid() {
  const ref = useStaggerReveal<HTMLDivElement>({ stagger: 80 });
  return (
    <div ref={ref} className="card-grid">
      {cards.map(c => <Card key={c.id} {...c} />)}
    </div>
  );
}
```

---

## 3. Count-up on Scroll

숫자가 0에서 목표값까지 올라가는 효과.

```tsx
// hooks/useCountUp.ts
import { useEffect, useRef, useState } from 'react';

interface CountUpOptions {
  target: number;
  duration?: number;       // ms, 기본 1200
  decimals?: number;       // 소수점 자리수
  separator?: string;      // 천 단위 구분자
  easing?: (t: number) => number;
}

// easeOutCubic
const defaultEasing = (t: number) => 1 - Math.pow(1 - t, 3);

export function useCountUp({ target, duration = 1200, decimals = 0, separator = ',', easing = defaultEasing }: CountUpOptions) {
  const [value, setValue] = useState(0);
  const ref = useRef<HTMLElement>(null);
  const startedRef = useRef(false);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (!entry.isIntersecting || startedRef.current) return;
        startedRef.current = true;

        if (mq.matches) { setValue(target); return; }

        const start = performance.now();
        const tick = (now: number) => {
          const progress = Math.min((now - start) / duration, 1);
          const current = target * easing(progress);
          setValue(current);
          if (progress < 1) requestAnimationFrame(tick);
        };
        requestAnimationFrame(tick);
        observer.unobserve(el);
      },
      { threshold: 0.5 }
    );

    observer.observe(el);
    return () => observer.disconnect();
  }, [target, duration, easing]);

  const formatted = value.toFixed(decimals).replace(/\B(?=(\d{3})+(?!\d))/g, separator);
  return { ref, value: formatted };
}
```

```tsx
// 사용 예시
function StatCard({ value, unit, label }: { value: number; unit: string; label: string }) {
  const { ref, value: displayed } = useCountUp({ target: value, duration: 1400 });
  return (
    <div>
      <span ref={ref as React.Ref<HTMLSpanElement>} aria-label={`${value}${unit}`}>
        {displayed}
      </span>
      <span aria-hidden="true">{unit}</span>
      <p>{label}</p>
    </div>
  );
}
```

---

## 주의사항

- `threshold: 0.15` — 요소의 15%가 보일 때 트리거. 너무 낮으면(0.01) 스크롤 시작 직후 모두 실행됨
- 페이지 로드 시 이미 뷰포트에 있는 요소 처리 — `rootMargin: '-1px'` 또는 초기 체크 추가
- 리스트 아이템 수가 많으면(50+) Observer를 container 하나에만 달고 자식에 딜레이 적용
