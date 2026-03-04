# Text Scramble

hover 또는 트리거 시 랜덤 문자로 바뀌었다가 원래 글자로 해독되는 효과.

## useScramble Hook

```tsx
// hooks/useScramble.ts
import { useCallback, useEffect, useRef, useState } from 'react';

const CHARS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*';

interface ScrambleOptions {
  duration?: number;      // 전체 해독 시간 ms, 기본 600
  fps?: number;           // 프레임 속도, 기본 20
  preserveSpaces?: boolean;
}

interface ScrambleChar {
  char: string;
  resolved: boolean;
}

export function useScramble(text: string, options: ScrambleOptions = {}) {
  const { duration = 600, fps = 20, preserveSpaces = true } = options;
  const [chars, setChars] = useState<ScrambleChar[]>(() =>
    text.split('').map(c => ({ char: c, resolved: true }))
  );
  const rafRef = useRef(0);
  const isRunningRef = useRef(false);

  const scramble = useCallback(() => {
    if (isRunningRef.current) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) return;  // 모션 감소 시 즉시 skip

    isRunningRef.current = true;
    const start = performance.now();
    const interval = 1000 / fps;
    let lastFrame = 0;

    function tick(now: number) {
      if (now - lastFrame < interval) { rafRef.current = requestAnimationFrame(tick); return; }
      lastFrame = now;

      const progress = Math.min((now - start) / duration, 1);

      setChars(text.split('').map((original, i) => {
        if (preserveSpaces && original === ' ') return { char: ' ', resolved: true };
        // 진행도에 따라 순차 해독 (왼쪽부터)
        const resolved = i / text.length < progress;
        return {
          char: resolved ? original : CHARS[Math.floor(Math.random() * CHARS.length)],
          resolved,
        };
      }));

      if (progress < 1) {
        rafRef.current = requestAnimationFrame(tick);
      } else {
        isRunningRef.current = false;
      }
    }

    rafRef.current = requestAnimationFrame(tick);
  }, [text, duration, fps, preserveSpaces]);

  // cleanup
  useEffect(() => () => cancelAnimationFrame(rafRef.current), []);

  return { chars, scramble };
}
```

## ScrambleText 컴포넌트

```tsx
// ScrambleText.tsx
import { useScramble } from '@/hooks/useScramble';
import styles from './ScrambleText.module.css';

interface ScrambleTextProps {
  text: string;
  trigger?: 'hover' | 'mount' | 'manual';
  duration?: number;
  fps?: number;
  className?: string;
  onScramble?: () => void;
}

export function ScrambleText({
  text,
  trigger = 'hover',
  duration,
  fps,
  className,
  onScramble,
}: ScrambleTextProps) {
  const { chars, scramble } = useScramble(text, { duration, fps });

  function handleTrigger() { scramble(); onScramble?.(); }

  useEffect(() => {
    if (trigger === 'mount') scramble();
  }, [trigger]);  // eslint-disable-line

  return (
    <span
      className={[styles.wrap, className].filter(Boolean).join(' ')}
      aria-label={text}
      onMouseEnter={trigger === 'hover' ? handleTrigger : undefined}
      onClick={trigger === 'manual' ? handleTrigger : undefined}
    >
      {chars.map((c, i) => (
        <span
          key={i}
          aria-hidden="true"
          className={c.resolved ? styles.resolved : styles.scrambling}
        >
          {c.char}
        </span>
      ))}
    </span>
  );
}
```

```css
/* ScrambleText.module.css */
.wrap { display: inline-block; cursor: default; font-variant-numeric: tabular-nums; }

.resolved {
  color: inherit;
  transition: color 50ms;
}

.scrambling {
  color: #6366f1;
  /* 고정폭 폰트로 레이아웃 흔들림 방지 */
  font-feature-settings: 'tnum';
}
```

### 사용 예시

```tsx
// hover 트리거
<ScrambleText text="god-damn-design" trigger="hover" />

// 페이지 로드 시 자동 실행
<ScrambleText text="Hello, World" trigger="mount" duration={800} />

// 버튼 클릭으로 수동 트리거
<button onClick={scramble}>
  <ScrambleText text="EXECUTE" trigger="manual" ref={...} />
</button>
```

---

## 주의사항

- `aria-label={text}` 필수 — 스크린리더는 랜덤 문자 읽지 않아야 함
- `font-variant-numeric: tabular-nums` — 숫자 자리가 달라지며 레이아웃 흔들림 방지
- 고정폭 폰트(`monospace`) 계열에서 가장 자연스러움 — proportional 폰트면 글자별 너비 차이로 흔들림 발생
- `duration`이 너무 짧으면(300ms 이하) 해독 효과 인지 어려움
