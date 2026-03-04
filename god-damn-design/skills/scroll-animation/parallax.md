# Parallax

스크롤 속도 차이로 깊이감을 만드는 패턴. RAF + Lerp로 부드럽게 처리.

## 1. useParallax Hook

```tsx
// hooks/useParallax.ts
import { useEffect, useRef } from 'react';

interface ParallaxOptions {
  speed?: number;         // 양수: 위로, 음수: 아래로. 범위: -1 ~ 1. 기본 0.2
  axis?: 'y' | 'x';
  lerp?: number;          // 보간 강도 0–1, 기본 0.08 (낮을수록 더 부드럽고 느림)
}

export function useParallax<T extends HTMLElement>(options: ParallaxOptions = {}) {
  const { speed = 0.2, axis = 'y', lerp = 0.08 } = options;
  const ref = useRef<T>(null);
  const currentRef = useRef(0);
  const targetRef = useRef(0);
  const rafRef = useRef(0);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) return;

    el.style.willChange = 'transform';

    function update() {
      targetRef.current = window.scrollY * speed * -1;
      currentRef.current += (targetRef.current - currentRef.current) * lerp;

      const delta = currentRef.current;
      el!.style.transform = axis === 'y'
        ? `translateY(${delta.toFixed(2)}px)`
        : `translateX(${delta.toFixed(2)}px)`;

      rafRef.current = requestAnimationFrame(update);
    }

    rafRef.current = requestAnimationFrame(update);

    return () => {
      cancelAnimationFrame(rafRef.current);
      el.style.willChange = '';
      el.style.transform = '';
    };
  }, [speed, axis, lerp]);

  return ref;
}
```

### 사용 예시

```tsx
function HeroSection() {
  const bgRef = useParallax<HTMLDivElement>({ speed: 0.15 });
  const textRef = useParallax<HTMLDivElement>({ speed: -0.05 });

  return (
    <section style={{ position: 'relative', overflow: 'hidden', height: '100vh' }}>
      {/* 배경 — 느리게 */}
      <div
        ref={bgRef}
        style={{
          position: 'absolute',
          inset: '-20%',          /* 패럴랙스 이동 여유 확보 */
          backgroundImage: 'url(...)',
          backgroundSize: 'cover',
        }}
        aria-hidden="true"
      />
      {/* 전경 텍스트 — 조금 더 빠르게 (반대 방향) */}
      <div ref={textRef} style={{ position: 'relative', zIndex: 1 }}>
        <h1>Parallax Hero</h1>
      </div>
    </section>
  );
}
```

---

## 2. 다층 Blob 패럴랙스

```tsx
// ParallaxBlobs.tsx
import { useParallax } from '@/hooks/useParallax';

interface BlobLayer {
  speed: number;
  color: string;
  size: number;
  top: string;
  left: string;
  blur: number;
}

const DEFAULT_LAYERS: BlobLayer[] = [
  { speed: 0.25,  color: '#6366f1', size: 500, top: '10%',  left: '5%',  blur: 80 },
  { speed: -0.15, color: '#a855f7', size: 400, top: '40%',  left: '60%', blur: 60 },
  { speed: 0.10,  color: '#ec4899', size: 300, top: '70%',  left: '20%', blur: 70 },
];

export function ParallaxBlobs({ layers = DEFAULT_LAYERS }: { layers?: BlobLayer[] }) {
  return (
    <div
      aria-hidden="true"
      style={{ position: 'absolute', inset: 0, overflow: 'hidden', pointerEvents: 'none' }}
    >
      {layers.map((layer, i) => (
        <BlobLayer key={i} {...layer} />
      ))}
    </div>
  );
}

function BlobLayer({ speed, color, size, top, left, blur }: BlobLayer) {
  const ref = useParallax<HTMLDivElement>({ speed });
  return (
    <div
      ref={ref}
      style={{
        position: 'absolute',
        top, left,
        width: size, height: size,
        borderRadius: '50%',
        background: color,
        filter: `blur(${blur}px)`,
        opacity: 0.25,
      }}
    />
  );
}
```

---

## 3. Scroll Progress Bar

```tsx
// ScrollProgressBar.tsx
import { useEffect, useRef } from 'react';

interface ScrollProgressBarProps {
  color?: string;
  height?: number;
  position?: 'top' | 'bottom';
}

export function ScrollProgressBar({
  color = 'linear-gradient(90deg, #6366f1, #a855f7, #ec4899)',
  height = 3,
  position = 'top',
}: ScrollProgressBarProps) {
  const barRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const bar = barRef.current;
    if (!bar) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');

    let rafId = 0;
    function update() {
      const scrolled = window.scrollY;
      const total = document.documentElement.scrollHeight - window.innerHeight;
      const pct = total > 0 ? (scrolled / total) * 100 : 0;
      bar!.style.transform = `scaleX(${pct / 100})`;
      rafId = requestAnimationFrame(update);
    }

    if (!mq.matches) rafId = requestAnimationFrame(update);

    return () => cancelAnimationFrame(rafId);
  }, []);

  return (
    <div
      aria-hidden="true"
      style={{
        position: 'fixed',
        [position]: 0,
        left: 0,
        width: '100%',
        height,
        zIndex: 9999,
        background: '#18181b',
      }}
    >
      <div
        ref={barRef}
        style={{
          width: '100%',
          height: '100%',
          background: color,
          transformOrigin: 'left center',
          transform: 'scaleX(0)',
          willChange: 'transform',
        }}
      />
    </div>
  );
}
```

---

## 주의사항

- `inset: '-20%'` — 패럴랙스 이동 범위만큼 컨테이너보다 크게 설정해야 가장자리 비지 않음
- `overflow: hidden`을 부모에 걸어야 삐져나온 영역 숨김
- `lerp` 값: `0.05` 이하는 너무 느려 반응 나쁨, `0.15` 이상은 부드럽지 않음 — `0.08` 권장
- 모바일에서 scroll 이벤트 성능 주의 — `passive: true` 옵션 (브라우저 기본 적용됨)
