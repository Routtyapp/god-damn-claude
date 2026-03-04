# Button Feedback

## 1. Squish Press Button

`:active` 시 scale + spring easing. layout 변경 없이 `transform`만 사용.

```tsx
// components/ui/Button.tsx
import { forwardRef, ButtonHTMLAttributes } from 'react';
import styles from './Button.module.css';

type Variant = 'primary' | 'ghost' | 'danger';
type Size = 'sm' | 'md' | 'lg';

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: Variant;
  size?: Size;
}

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = 'primary', size = 'md', className, children, ...props }, ref) => (
    <button
      ref={ref}
      className={[styles.btn, styles[variant], styles[size], className].filter(Boolean).join(' ')}
      {...props}
    >
      {children}
    </button>
  )
);
Button.displayName = 'Button';
```

```css
/* Button.module.css */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  border: none;
  border-radius: 10px;
  font-weight: 600;
  cursor: pointer;
  user-select: none;
  /* transform만 사용 — layout 변화 없음 */
  transition:
    transform 80ms cubic-bezier(0.34, 1.56, 0.64, 1),
    box-shadow 200ms ease,
    background-color 150ms ease;
  will-change: transform;
}

/* Size */
.sm { padding: 8px 16px;  font-size: 13px; }
.md { padding: 12px 24px; font-size: 14px; }
.lg { padding: 16px 32px; font-size: 16px; }

/* Variants */
.primary {
  background: #6366f1;
  color: #fff;
  box-shadow: 0 0 0 0 #6366f140;
}
.primary:hover {
  background: #4f46e5;
  box-shadow: 0 0 0 4px #6366f125;
}
/* :active는 hover보다 먼저 선언 — specificity 동일시 순서 보장 */
.primary:active { transform: scale(0.93); box-shadow: 0 0 0 0 #6366f140; }

.ghost {
  background: transparent;
  color: #a1a1aa;
  box-shadow: inset 0 0 0 1.5px #27272a;
}
.ghost:hover { color: #6366f1; box-shadow: inset 0 0 0 1.5px #6366f1; }
.ghost:active { transform: scale(0.93); }

.danger { background: #ef4444; color: #fff; }
.danger:hover { background: #dc2626; }
.danger:active { transform: scale(0.93); }

/* Disabled */
.btn:disabled { opacity: 0.4; cursor: not-allowed; pointer-events: none; }

/* Accessibility — prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  .btn { transition: background-color 150ms ease, box-shadow 150ms ease; }
  .btn:active { transform: none; }
}
```

---

## 2. Ripple Button

클릭 위치 기준으로 원이 퍼지는 효과. 클립 마스크 대신 overflow:hidden 사용.

```tsx
// hooks/useRipple.ts
import { useCallback, useRef } from 'react';

interface RippleOptions {
  color?: string;
  duration?: number;
}

export function useRipple({ color = 'rgba(255,255,255,0.35)', duration = 500 }: RippleOptions = {}) {
  const containerRef = useRef<HTMLElement>(null);

  const trigger = useCallback((e: React.MouseEvent) => {
    const el = containerRef.current;
    if (!el) return;

    const rect = el.getBoundingClientRect();
    const size = Math.max(el.offsetWidth, el.offsetHeight) * 2;
    const x = e.clientX - rect.left - size / 2;
    const y = e.clientY - rect.top - size / 2;

    const ripple = document.createElement('span');
    ripple.style.cssText = `
      position:absolute; pointer-events:none; border-radius:50%;
      width:${size}px; height:${size}px;
      left:${x}px; top:${y}px;
      background:${color};
      transform:scale(0); opacity:1;
      animation:ripple-expand ${duration}ms cubic-bezier(0,0,0.2,1) forwards;
    `;
    el.appendChild(ripple);
    ripple.addEventListener('animationend', () => ripple.remove(), { once: true });
  }, [color, duration]);

  return { containerRef, trigger };
}
```

```css
/* globals.css — 전역 1회 선언 */
@keyframes ripple-expand {
  to { transform: scale(1); opacity: 0; }
}
```

```tsx
// RippleButton.tsx
import { forwardRef, ButtonHTMLAttributes } from 'react';
import { useRipple } from '@/hooks/useRipple';

interface RippleButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  rippleColor?: string;
}

export const RippleButton = forwardRef<HTMLButtonElement, RippleButtonProps>(
  ({ rippleColor, onClick, children, style, ...props }, _ref) => {
    const { containerRef, trigger } = useRipple({ color: rippleColor });

    return (
      <button
        ref={containerRef as React.Ref<HTMLButtonElement>}
        onClick={(e) => { trigger(e); onClick?.(e); }}
        style={{ position: 'relative', overflow: 'hidden', ...style }}
        {...props}
      >
        {children}
      </button>
    );
  }
);
RippleButton.displayName = 'RippleButton';
```

---

## 3. Magnetic Hover Button

마우스가 가까워지면 버튼이 따라오는 효과. `requestAnimationFrame` + Lerp.

```tsx
// MagneticButton.tsx
import { useRef, useEffect, ReactNode } from 'react';

interface MagneticButtonProps {
  children: ReactNode;
  strength?: number;  // 당기는 강도 (0–1)
  className?: string;
}

export function MagneticButton({ children, strength = 0.35, className }: MagneticButtonProps) {
  const btnRef = useRef<HTMLButtonElement>(null);
  const posRef = useRef({ x: 0, y: 0 });
  const rafRef = useRef<number>(0);

  useEffect(() => {
    const btn = btnRef.current;
    if (!btn) return;

    // prefers-reduced-motion 체크
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) return;

    let isHovered = false;

    function lerp(a: number, b: number, t: number) {
      return a + (b - a) * t;
    }

    function animate() {
      posRef.current.x = lerp(posRef.current.x, isHovered ? posRef.current.x : 0, 0.12);
      posRef.current.y = lerp(posRef.current.y, isHovered ? posRef.current.y : 0, 0.12);
      btn!.style.transform = `translate(${posRef.current.x}px, ${posRef.current.y}px)`;
      rafRef.current = requestAnimationFrame(animate);
    }

    function onMouseMove(e: MouseEvent) {
      const rect = btn!.getBoundingClientRect();
      const cx = rect.left + rect.width / 2;
      const cy = rect.top + rect.height / 2;
      posRef.current = {
        x: (e.clientX - cx) * strength,
        y: (e.clientY - cy) * strength,
      };
    }

    function onMouseEnter() { isHovered = true; btn!.addEventListener('mousemove', onMouseMove); }
    function onMouseLeave() {
      isHovered = false;
      btn!.removeEventListener('mousemove', onMouseMove);
      // 원위치 복귀
      posRef.current = { x: 0, y: 0 };
    }

    btn.addEventListener('mouseenter', onMouseEnter);
    btn.addEventListener('mouseleave', onMouseLeave);
    rafRef.current = requestAnimationFrame(animate);

    return () => {
      cancelAnimationFrame(rafRef.current);
      btn.removeEventListener('mouseenter', onMouseEnter);
      btn.removeEventListener('mouseleave', onMouseLeave);
      btn.removeEventListener('mousemove', onMouseMove);
    };
  }, [strength]);

  return (
    <button ref={btnRef} className={className} style={{ willChange: 'transform' }}>
      {children}
    </button>
  );
}
```

---

## 공통 주의사항

- `transform: scale()` 은 인접 요소 레이아웃에 영향을 주지 않는다 — `width/height` 변경 금지
- `:active` 딜레이가 발생하면 `touch-action: manipulation` 추가
- 아이콘만 있는 버튼은 `aria-label` 필수
