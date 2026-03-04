# Typewriter

## useTypewriter Hook

```tsx
// hooks/useTypewriter.ts
import { useEffect, useRef, useState } from 'react';

interface TypewriterOptions {
  phrases: string[];
  typeSpeed?: number;     // ms per char, 기본 75
  deleteSpeed?: number;   // ms per char, 기본 40
  pauseAfter?: number;    // 완성 후 대기 ms, 기본 1800
  loop?: boolean;         // 기본 true
}

interface TypewriterState {
  text: string;
  isTyping: boolean;      // true: 타이핑 중, false: 삭제 중
  phraseIndex: number;
}

export function useTypewriter({
  phrases,
  typeSpeed = 75,
  deleteSpeed = 40,
  pauseAfter = 1800,
  loop = true,
}: TypewriterOptions) {
  const [state, setState] = useState<TypewriterState>({
    text: '',
    isTyping: true,
    phraseIndex: 0,
  });
  const timerRef = useRef<ReturnType<typeof setTimeout>>();

  useEffect(() => {
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    // 모션 감소 시 전체 문장 즉시 표시
    if (mq.matches) {
      setState({ text: phrases[0], isTyping: false, phraseIndex: 0 });
      return;
    }

    const phrase = phrases[state.phraseIndex];

    if (state.isTyping) {
      if (state.text.length < phrase.length) {
        timerRef.current = setTimeout(() => {
          setState(s => ({ ...s, text: phrase.slice(0, s.text.length + 1) }));
        }, typeSpeed);
      } else {
        // 완성 → 대기 → 삭제 모드 전환
        timerRef.current = setTimeout(() => {
          setState(s => ({ ...s, isTyping: false }));
        }, pauseAfter);
      }
    } else {
      if (state.text.length > 0) {
        timerRef.current = setTimeout(() => {
          setState(s => ({ ...s, text: s.text.slice(0, -1) }));
        }, deleteSpeed);
      } else {
        // 다음 문장으로
        const nextIndex = loop
          ? (state.phraseIndex + 1) % phrases.length
          : Math.min(state.phraseIndex + 1, phrases.length - 1);
        setState({ text: '', isTyping: true, phraseIndex: nextIndex });
      }
    }

    return () => clearTimeout(timerRef.current);
  }, [state, phrases, typeSpeed, deleteSpeed, pauseAfter, loop]);

  return { text: state.text, isTyping: state.isTyping };
}
```

## TypewriterText 컴포넌트

```tsx
// TypewriterText.tsx
import { useTypewriter } from '@/hooks/useTypewriter';
import styles from './TypewriterText.module.css';

interface TypewriterTextProps {
  phrases: string[];
  prefix?: string;        // 고정 앞 텍스트
  typeSpeed?: number;
  deleteSpeed?: number;
  pauseAfter?: number;
  className?: string;
}

export function TypewriterText({
  phrases,
  prefix,
  typeSpeed,
  deleteSpeed,
  pauseAfter,
  className,
}: TypewriterTextProps) {
  const { text, isTyping } = useTypewriter({ phrases, typeSpeed, deleteSpeed, pauseAfter });
  // 스크린리더에는 phrases 전체를 숨겨진 텍스트로 제공
  const fullLabel = [prefix, phrases.join(' / ')].filter(Boolean).join(' ');

  return (
    <span className={[styles.wrap, className].filter(Boolean).join(' ')} aria-label={fullLabel}>
      {prefix && <span aria-hidden="true">{prefix}</span>}
      <span aria-hidden="true" className={styles.typed}>{text}</span>
      <span
        aria-hidden="true"
        className={styles.cursor}
        data-typing={isTyping}
      />
    </span>
  );
}
```

```css
/* TypewriterText.module.css */
.wrap { display: inline-flex; align-items: baseline; gap: 6px; }
.typed { font-variant-numeric: tabular-nums; }

.cursor {
  display: inline-block;
  width: 2px;
  height: 1.1em;
  background: currentColor;
  border-radius: 1px;
  animation: cursor-blink 700ms step-end infinite;
  vertical-align: text-bottom;
  margin-left: 2px;
  flex-shrink: 0;
}

/* 삭제 중에는 커서 항상 보임 */
.cursor[data-typing="false"] { animation: none; opacity: 1; }

@keyframes cursor-blink {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0; }
}

@media (prefers-reduced-motion: reduce) {
  .cursor { animation: none; }
}
```

### 사용 예시

```tsx
// 단순 타이핑
<TypewriterText
  phrases={['인터랙션을 설계합니다.', 'UI를 빌드합니다.', '경험을 만듭니다.']}
  typeSpeed={70}
  deleteSpeed={35}
  pauseAfter={2000}
/>

// prefix 포함
<h1>
  <TypewriterText
    prefix="우리는"
    phrases={['디자인합니다.', '개발합니다.', '혁신합니다.']}
  />
</h1>
```

---

## 주의사항

- `phrases` 배열이 변경되면 hook이 재시작됨 — 컴포넌트 외부에 상수로 선언
- `deleteSpeed < typeSpeed` 권장 — 삭제가 빠를수록 자연스러움
- 단일 문장(loop=false)이면 삭제 없이 멈추도록 로직 처리 필요
