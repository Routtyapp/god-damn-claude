# Form Feedback

## 1. Floating Label Input

라벨이 placeholder 역할을 하다가 focus 또는 입력 시 상단으로 이동.

```tsx
// FloatingInput.tsx
import { forwardRef, InputHTMLAttributes, useId } from 'react';
import styles from './FloatingInput.module.css';

interface FloatingInputProps extends InputHTMLAttributes<HTMLInputElement> {
  label: string;
  error?: string;
  hint?: string;
}

export const FloatingInput = forwardRef<HTMLInputElement, FloatingInputProps>(
  ({ label, error, hint, className, id, ...props }, ref) => {
    const autoId = useId();
    const inputId = id ?? autoId;
    const hintId = `${inputId}-hint`;
    const errorId = `${inputId}-error`;

    return (
      <div className={[styles.field, error && styles.hasError, className].filter(Boolean).join(' ')}>
        <input
          ref={ref}
          id={inputId}
          placeholder=" "   /* placeholder=" " 필수 — :placeholder-shown 트리거 */
          aria-describedby={[error && errorId, hint && hintId].filter(Boolean).join(' ') || undefined}
          aria-invalid={!!error}
          className={styles.input}
          {...props}
        />
        <label htmlFor={inputId} className={styles.label}>{label}</label>
        <span className={styles.focusLine} aria-hidden="true" />
        {hint && !error && <span id={hintId} className={styles.hint}>{hint}</span>}
        {error && <span id={errorId} className={styles.error} role="alert">{error}</span>}
      </div>
    );
  }
);
FloatingInput.displayName = 'FloatingInput';
```

```css
/* FloatingInput.module.css */
.field {
  position: relative;
  width: 100%;
}

.input {
  width: 100%;
  padding: 22px 16px 8px;   /* top padding 확보 — 라벨 공간 */
  background: #18181b;
  border: 1.5px solid #27272a;
  border-radius: 10px;
  color: #e4e4f0;
  font-size: 15px;
  outline: none;
  transition: border-color 180ms ease;
  /* 입력값이 있을 때 bottom border 표시용 */
}

.input:focus { border-color: #6366f1; }
.hasError .input { border-color: #ef4444; }

/* Floating label */
.label {
  position: absolute;
  left: 16px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 15px;
  color: #52525b;
  pointer-events: none;
  transform-origin: left center;
  transition:
    top 160ms cubic-bezier(0.16, 1, 0.3, 1),
    font-size 160ms cubic-bezier(0.16, 1, 0.3, 1),
    color 160ms ease,
    transform 160ms cubic-bezier(0.16, 1, 0.3, 1);
}

/* 조건: focus OR 값 있음(:placeholder-shown의 반대) */
.input:focus ~ .label,
.input:not(:placeholder-shown) ~ .label {
  top: 10px;
  transform: translateY(0) scale(0.78);
  color: #6366f1;
}

.hasError .input:focus ~ .label,
.hasError .input:not(:placeholder-shown) ~ .label {
  color: #ef4444;
}

/* Animated focus underline */
.focusLine {
  position: absolute;
  bottom: 0; left: 50%;
  width: 0;
  height: 2px;
  background: #6366f1;
  border-radius: 0 0 10px 10px;
  transform: translateX(-50%);
  transition: width 200ms cubic-bezier(0.16, 1, 0.3, 1);
}
.input:focus ~ .label ~ .focusLine { width: calc(100% - 2px); }
.hasError .input:focus ~ .label ~ .focusLine { background: #ef4444; }

.hint  { display: block; margin-top: 6px; font-size: 12px; color: #52525b; padding-left: 4px; }
.error { display: block; margin-top: 6px; font-size: 12px; color: #ef4444; padding-left: 4px; }

@media (prefers-reduced-motion: reduce) {
  .label, .focusLine { transition: none; }
}
```

---

## 2. Focus Ring (전역 유틸리티)

마우스 사용자에겐 숨기고, 키보드 사용자에게만 표시.

```css
/* globals.css */

/* 기본 outline 제거 */
*:focus { outline: none; }

/* 키보드 포커스에만 표시 */
*:focus-visible {
  outline: 2px solid #6366f1;
  outline-offset: 3px;
  border-radius: 4px;  /* 요소마다 override 가능 */
}

/* 카드/섹션처럼 border-radius가 큰 요소 */
.focus-lg:focus-visible { border-radius: 12px; }
```

---

## 3. Validation State Input

성공/실패 상태를 아이콘 + 색상으로 표현.

```tsx
// ValidatedInput.tsx
type ValidationState = 'idle' | 'success' | 'error' | 'loading';

interface ValidatedInputProps extends InputHTMLAttributes<HTMLInputElement> {
  label: string;
  state?: ValidationState;
  message?: string;
}

const icons: Record<ValidationState, string | null> = {
  idle: null,
  loading: '⟳',
  success: '✓',
  error: '✕',
};

export function ValidatedInput({ label, state = 'idle', message, ...props }: ValidatedInputProps) {
  return (
    <div className={styles.field} data-state={state}>
      <FloatingInput label={label} error={state === 'error' ? message : undefined} {...props} />
      {state !== 'idle' && (
        <span
          className={styles.stateIcon}
          aria-hidden="true"
          data-state={state}
        >
          {icons[state]}
        </span>
      )}
      {state === 'success' && message && (
        <span className={styles.successMsg}>{message}</span>
      )}
    </div>
  );
}
```

```css
/* 상태 아이콘 */
.stateIcon {
  position: absolute;
  right: 14px; top: 50%; transform: translateY(-50%);
  font-size: 14px;
  transition: opacity 200ms;
}
[data-state="success"] .stateIcon { color: #22c55e; }
[data-state="error"]   .stateIcon { color: #ef4444; }
[data-state="loading"] .stateIcon {
  color: #6366f1;
  animation: spin 600ms linear infinite;
}
@keyframes spin { to { transform: translateY(-50%) rotate(360deg); } }

.successMsg { display: block; margin-top: 6px; font-size: 12px; color: #22c55e; padding-left: 4px; }
```

---

## 공통 주의사항

- `placeholder=" "` (공백 한 칸) 필수 — `:placeholder-shown` pseudo-class 트리거
- `aria-invalid`, `aria-describedby`로 스크린리더 에러 연결 필수
- `role="alert"`는 에러 메시지에만 — 너무 많이 쓰면 노이즈
