# Toggle & Checkbox

## 1. Toggle Switch (Spring)

```tsx
// ToggleSwitch.tsx
import { forwardRef, InputHTMLAttributes } from 'react';
import styles from './ToggleSwitch.module.css';

interface ToggleSwitchProps extends Omit<InputHTMLAttributes<HTMLInputElement>, 'type' | 'size'> {
  label?: string;
  size?: 'sm' | 'md';
}

export const ToggleSwitch = forwardRef<HTMLInputElement, ToggleSwitchProps>(
  ({ label, size = 'md', id, className, ...props }, ref) => {
    const inputId = id ?? `toggle-${Math.random().toString(36).slice(2)}`;

    return (
      <label htmlFor={inputId} className={[styles.wrap, styles[size], className].filter(Boolean).join(' ')}>
        <input ref={ref} id={inputId} type="checkbox" role="switch" className={styles.input} {...props} />
        <span className={styles.track} aria-hidden="true">
          <span className={styles.thumb} />
        </span>
        {label && <span className={styles.label}>{label}</span>}
      </label>
    );
  }
);
ToggleSwitch.displayName = 'ToggleSwitch';
```

```css
/* ToggleSwitch.module.css */
.wrap {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  user-select: none;
}

.input {
  position: absolute;
  opacity: 0;
  width: 0; height: 0;
  pointer-events: none;
}

/* Track */
.track {
  position: relative;
  display: block;
  background: #27272a;
  border-radius: 99px;
  transition: background 250ms cubic-bezier(0.34, 1.56, 0.64, 1);
  flex-shrink: 0;
}
.md .track { width: 52px; height: 28px; }
.sm .track { width: 40px; height: 22px; }

/* Thumb */
.thumb {
  position: absolute;
  top: 3px; left: 3px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 1px 4px rgba(0,0,0,0.25);
  transition: transform 280ms cubic-bezier(0.34, 1.56, 0.64, 1),
              width 120ms ease;
}
.md .thumb { width: 22px; height: 22px; }
.sm .thumb { width: 16px; height: 16px; }

/* Checked state */
.input:checked ~ .track { background: #6366f1; }
.md .input:checked ~ .track .thumb { transform: translateX(24px); }
.sm .input:checked ~ .track .thumb { transform: translateX(18px); }

/* Press — thumb squish */
.wrap:active .thumb {
  width: 26px; /* md 기준 */
}
.wrap:active .input:checked ~ .track .thumb {
  transform: translateX(20px);
}

/* Focus visible */
.input:focus-visible ~ .track {
  outline: 2px solid #6366f1;
  outline-offset: 2px;
}

/* Disabled */
.input:disabled ~ .track { opacity: 0.4; }
.wrap:has(.input:disabled) { cursor: not-allowed; }

/* Label */
.label { font-size: 14px; color: #a1a1aa; transition: color 200ms; }
.wrap:has(.input:checked) .label { color: #e4e4f0; }

@media (prefers-reduced-motion: reduce) {
  .track, .thumb { transition: none; }
}
```

### 사용 예시

```tsx
// 비제어 (기본)
<ToggleSwitch label="다크 모드" defaultChecked />

// 제어 (controlled)
const [on, setOn] = useState(false);
<ToggleSwitch
  label="알림"
  checked={on}
  onChange={e => setOn(e.target.checked)}
/>
```

---

## 2. Animated Checkbox

```tsx
// Checkbox.tsx
import { forwardRef, InputHTMLAttributes, useId } from 'react';
import styles from './Checkbox.module.css';

interface CheckboxProps extends Omit<InputHTMLAttributes<HTMLInputElement>, 'type'> {
  label?: string;
}

export const Checkbox = forwardRef<HTMLInputElement, CheckboxProps>(
  ({ label, className, ...props }, ref) => {
    const id = useId();

    return (
      <label htmlFor={id} className={[styles.wrap, className].filter(Boolean).join(' ')}>
        <input ref={ref} id={id} type="checkbox" className={styles.input} {...props} />
        <span className={styles.box} aria-hidden="true">
          {/* SVG 체크마크 — CSS transition으로 stroke-dashoffset 애니메이션 */}
          <svg viewBox="0 0 12 10" fill="none" className={styles.check}>
            <path
              d="M1 5l3.5 3.5L11 1"
              stroke="currentColor"
              strokeWidth="2"
              strokeLinecap="round"
              strokeLinejoin="round"
              className={styles.checkPath}
            />
          </svg>
        </span>
        {label && <span className={styles.label}>{label}</span>}
      </label>
    );
  }
);
Checkbox.displayName = 'Checkbox';
```

```css
/* Checkbox.module.css */
.wrap {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  user-select: none;
}

.input {
  position: absolute;
  opacity: 0;
  width: 0; height: 0;
}

.box {
  width: 20px; height: 20px;
  border: 1.5px solid #3f3f46;
  border-radius: 6px;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
  background: transparent;
  color: #fff;
  transition:
    background 200ms cubic-bezier(0.34, 1.56, 0.64, 1),
    border-color 200ms,
    transform 150ms cubic-bezier(0.34, 1.56, 0.64, 1);
}

.check { width: 12px; height: 10px; overflow: visible; }

/* stroke-dasharray/dashoffset으로 체크마크 그리기 애니메이션 */
.checkPath {
  stroke-dasharray: 16;
  stroke-dashoffset: 16;
  transition: stroke-dashoffset 220ms cubic-bezier(0.16, 1, 0.3, 1) 50ms;
}

/* Checked */
.input:checked ~ .box {
  background: #6366f1;
  border-color: #6366f1;
  transform: scale(1.08);
}
.input:checked ~ .box .checkPath {
  stroke-dashoffset: 0;
}

/* Focus visible */
.input:focus-visible ~ .box {
  outline: 2px solid #6366f1;
  outline-offset: 2px;
}

/* Hover */
.wrap:hover .box { border-color: #6366f1; }

/* Disabled */
.input:disabled ~ .box { opacity: 0.4; }
.wrap:has(.input:disabled) { cursor: not-allowed; }

.label { font-size: 14px; color: #a1a1aa; }

@media (prefers-reduced-motion: reduce) {
  .box, .checkPath { transition: none; }
  .checkPath { stroke-dashoffset: 0; }
}
```
