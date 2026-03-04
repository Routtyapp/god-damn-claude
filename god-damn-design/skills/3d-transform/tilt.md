# Perspective Tilt

마우스 위치에 따라 요소가 3D로 기울어지고 spotlight이 이동하는 효과.

## useTilt Hook

```tsx
// hooks/useTilt.ts
import { useEffect, useRef, RefObject } from 'react';

interface TiltOptions {
  max?: number;          // 최대 기울기 각도(deg), 기본 12
  perspective?: number;  // 기본 800
  scale?: number;        // hover 시 확대 배율, 기본 1.02
  lerp?: number;         // 보간 강도, 기본 0.12
  spotlight?: boolean;   // 조명 효과, 기본 true
}

interface TiltState {
  rotX: number;
  rotY: number;
  x: number;    // 0–1 마우스 x 비율
  y: number;    // 0–1 마우스 y 비율
}

export function useTilt<T extends HTMLElement>(options: TiltOptions = {}): RefObject<T> {
  const {
    max = 12,
    perspective = 800,
    scale = 1.02,
    lerp = 0.12,
    spotlight = true,
  } = options;

  const ref = useRef<T>(null);
  const stateRef = useRef<TiltState>({ rotX: 0, rotY: 0, x: 0.5, y: 0.5 });
  const targetRef = useRef<TiltState>({ rotX: 0, rotY: 0, x: 0.5, y: 0.5 });
  const rafRef = useRef(0);
  const isHoveredRef = useRef(false);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) return;

    function lerpVal(a: number, b: number, t: number) { return a + (b - a) * t; }

    function animate() {
      const s = stateRef.current;
      const t = targetRef.current;

      s.rotX = lerpVal(s.rotX, t.rotX, lerp);
      s.rotY = lerpVal(s.rotY, t.rotY, lerp);
      s.x   = lerpVal(s.x, t.x, lerp);
      s.y   = lerpVal(s.y, t.y, lerp);

      const currentScale = isHoveredRef.current ? scale : 1;

      el!.style.transform = [
        `perspective(${perspective}px)`,
        `rotateX(${s.rotX.toFixed(3)}deg)`,
        `rotateY(${s.rotY.toFixed(3)}deg)`,
        `scale(${currentScale})`,
      ].join(' ');

      if (spotlight) {
        el!.style.setProperty('--spotlight-x', `${(s.x * 100).toFixed(1)}%`);
        el!.style.setProperty('--spotlight-y', `${(s.y * 100).toFixed(1)}%`);
        el!.style.setProperty('--spotlight-opacity', isHoveredRef.current ? '1' : '0');
      }

      rafRef.current = requestAnimationFrame(animate);
    }

    function onMouseMove(e: MouseEvent) {
      const rect = el!.getBoundingClientRect();
      const x = (e.clientX - rect.left) / rect.width;
      const y = (e.clientY - rect.top) / rect.height;
      targetRef.current = {
        rotX: (0.5 - y) * max * 2,
        rotY: (x - 0.5) * max * 2,
        x, y,
      };
    }

    function onMouseEnter() {
      isHoveredRef.current = true;
      el!.style.willChange = 'transform';
    }

    function onMouseLeave() {
      isHoveredRef.current = false;
      targetRef.current = { rotX: 0, rotY: 0, x: 0.5, y: 0.5 };
    }

    el.addEventListener('mousemove', onMouseMove);
    el.addEventListener('mouseenter', onMouseEnter);
    el.addEventListener('mouseleave', onMouseLeave);
    rafRef.current = requestAnimationFrame(animate);

    return () => {
      cancelAnimationFrame(rafRef.current);
      el.removeEventListener('mousemove', onMouseMove);
      el.removeEventListener('mouseenter', onMouseEnter);
      el.removeEventListener('mouseleave', onMouseLeave);
      el.style.transform = '';
      el.style.willChange = '';
    };
  }, [max, perspective, scale, lerp, spotlight]);

  return ref;
}
```

## TiltCard 컴포넌트

```tsx
// TiltCard.tsx
import { ReactNode } from 'react';
import { useTilt } from '@/hooks/useTilt';
import styles from './TiltCard.module.css';

interface TiltCardProps {
  children: ReactNode;
  className?: string;
  tiltOptions?: Parameters<typeof useTilt>[0];
}

export function TiltCard({ children, className, tiltOptions }: TiltCardProps) {
  const ref = useTilt<HTMLDivElement>(tiltOptions);
  return (
    <div ref={ref} className={[styles.card, className].filter(Boolean).join(' ')}>
      {/* spotlight 레이어 */}
      <div className={styles.spotlight} aria-hidden="true" />
      <div className={styles.content}>{children}</div>
    </div>
  );
}
```

```css
/* TiltCard.module.css */
.card {
  position: relative;
  border-radius: 16px;
  overflow: hidden;
  /* transform은 useTilt가 인라인으로 주입 */
  transition: box-shadow 300ms ease;
}

.card:hover {
  box-shadow:
    0 30px 80px rgba(0,0,0,0.4),
    0 0 0 1px rgba(99,102,241,0.15);
}

/* Spotlight — CSS 변수로 위치 제어 */
.spotlight {
  position: absolute;
  inset: 0;
  pointer-events: none;
  background: radial-gradient(
    circle at var(--spotlight-x, 50%) var(--spotlight-y, 50%),
    rgba(255,255,255,0.12),
    transparent 60%
  );
  opacity: var(--spotlight-opacity, 0);
  transition: opacity 300ms ease;
}

.content {
  position: relative;
  z-index: 1;
  /* 자식 요소에 translateZ로 깊이감 추가 가능 */
  transform-style: preserve-3d;
}
```

### 사용 예시

```tsx
<TiltCard
  tiltOptions={{ max: 10, spotlight: true }}
  style={{ width: 280, height: 360, background: '#18181b', padding: 28 }}
>
  <h3 style={{ transform: 'translateZ(20px)' }}>카드 제목</h3>
  <p style={{ transform: 'translateZ(10px)' }}>카드 설명</p>
</TiltCard>
```

---

## 주의사항

- `transform-style: preserve-3d`를 `.content`에 걸어야 자식 `translateZ` 동작
- `overflow: hidden`과 `transform-style: preserve-3d`는 같은 요소에 공존 불가 — 레이어 분리 필수
- `will-change: transform` — mouseenter에서만 추가, mouseleave에서 제거
- 모바일: touchmove로 동일 효과 구현 가능하나 성능 주의 — `pointer: coarse` 미디어 쿼리로 비활성화 권장
