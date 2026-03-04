# Character & Word Reveal

## 1. CharReveal — 글자별 translateY 등장

```tsx
// CharReveal.tsx
import { useEffect, useRef } from 'react';
import styles from './CharReveal.module.css';

interface CharRevealProps {
  text: string;
  stagger?: number;       // 글자 간 딜레이 ms, 기본 35
  duration?: number;      // 개별 글자 전환 ms, 기본 500
  delay?: number;         // 전체 시작 딜레이 ms, 기본 0
  trigger?: 'mount' | 'inview';
  className?: string;
}

export function CharReveal({
  text,
  stagger = 35,
  duration = 500,
  delay = 0,
  trigger = 'inview',
  className,
}: CharRevealProps) {
  const wrapRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const wrap = wrapRef.current;
    if (!wrap) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) {
      wrap.classList.add(styles.revealed);
      return;
    }

    if (trigger === 'mount') {
      requestAnimationFrame(() => wrap.classList.add(styles.revealed));
      return;
    }

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          wrap.classList.add(styles.revealed);
          observer.unobserve(wrap);
        }
      },
      { threshold: 0.3 }
    );
    observer.observe(wrap);
    return () => observer.disconnect();
  }, [trigger]);

  // 단어 단위로 분리 — 줄바꿈 처리
  const words = text.split(' ');
  let charIndex = 0;

  return (
    <div
      ref={wrapRef}
      className={[styles.wrap, className].filter(Boolean).join(' ')}
      aria-label={text}
    >
      {words.map((word, wi) => (
        <span key={wi} className={styles.word}>
          {word.split('').map((char, ci) => {
            const idx = charIndex++;
            return (
              <span
                key={ci}
                aria-hidden="true"
                className={styles.char}
                style={{
                  '--delay': `${delay + idx * stagger}ms`,
                  '--duration': `${duration}ms`,
                } as React.CSSProperties}
              >
                {char}
              </span>
            );
          })}
          {wi < words.length - 1 && (
            <span aria-hidden="true" className={styles.space}>&nbsp;</span>
          )}
        </span>
      ))}
    </div>
  );
}
```

```css
/* CharReveal.module.css */
.wrap { display: flex; flex-wrap: wrap; overflow: hidden; }
.word { display: inline-flex; overflow: hidden; }
.space { display: inline-block; width: 0.3em; }

.char {
  display: inline-block;
  transform: translateY(110%);
  opacity: 0;
  transition:
    transform var(--duration, 500ms) cubic-bezier(0.16, 1, 0.3, 1) var(--delay, 0ms),
    opacity   var(--duration, 500ms) ease                          var(--delay, 0ms);
}

.revealed .char {
  transform: translateY(0);
  opacity: 1;
}

@media (prefers-reduced-motion: reduce) {
  .char { transition: none; transform: none; opacity: 1; }
}
```

---

## 2. WordRotate — 단어 슬라이드 전환

```tsx
// WordRotate.tsx
import { useEffect, useState } from 'react';
import styles from './WordRotate.module.css';

interface WordRotateProps {
  words: string[];
  interval?: number;      // 전환 간격 ms, 기본 2500
  className?: string;
}

export function WordRotate({ words, interval = 2500, className }: WordRotateProps) {
  const [current, setCurrent] = useState(0);
  const [prev, setPrev] = useState<number | null>(null);

  useEffect(() => {
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) return;

    const id = setInterval(() => {
      setCurrent(c => {
        const next = (c + 1) % words.length;
        setPrev(c);
        // 이전 항목 클래스 제거
        setTimeout(() => setPrev(null), 400);
        return next;
      });
    }, interval);

    return () => clearInterval(id);
  }, [words.length, interval]);

  return (
    <span
      className={[styles.wrap, className].filter(Boolean).join(' ')}
      aria-live="polite"
      aria-atomic="true"
    >
      {words.map((word, i) => (
        <span
          key={word}
          aria-hidden={i !== current}
          className={[
            styles.item,
            i === current ? styles.active : '',
            i === prev    ? styles.exiting : '',
          ].filter(Boolean).join(' ')}
        >
          {word}
        </span>
      ))}
    </span>
  );
}
```

```css
/* WordRotate.module.css */
.wrap {
  position: relative;
  display: inline-block;
  overflow: hidden;
  /* 가장 긴 단어 기준 너비 확보 필요시 min-width 설정 */
}

.item {
  display: block;
  position: absolute;
  top: 100%;      /* 초기: 아래 대기 */
  left: 0;
  white-space: nowrap;
  opacity: 0;
  transition:
    top     400ms cubic-bezier(0.16, 1, 0.3, 1),
    opacity 300ms ease;
}

.active {
  position: relative;
  top: 0;
  opacity: 1;
}

.exiting {
  top: -100%;     /* 위로 사라짐 */
  opacity: 0;
}

@media (prefers-reduced-motion: reduce) {
  .item { transition: none; }
  .item:not(.active) { display: none; }
}
```

---

## 3. BlurReveal — blur 해제로 등장

```tsx
// BlurReveal.tsx
import { useEffect, useRef } from 'react';
import styles from './BlurReveal.module.css';

interface BlurRevealProps {
  children: React.ReactNode;
  delay?: number;
  duration?: number;
  className?: string;
}

export function BlurReveal({ children, delay = 0, duration = 800, className }: BlurRevealProps) {
  const ref = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;

    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if (mq.matches) { el.style.opacity = '1'; el.style.filter = 'none'; return; }

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          el.classList.add(styles.revealed);
          observer.unobserve(el);
        }
      },
      { threshold: 0.2 }
    );
    observer.observe(el);
    return () => observer.disconnect();
  }, []);

  return (
    <div
      ref={ref}
      className={[styles.wrap, className].filter(Boolean).join(' ')}
      style={{
        '--delay': `${delay}ms`,
        '--duration': `${duration}ms`,
      } as React.CSSProperties}
    >
      {children}
    </div>
  );
}
```

```css
/* BlurReveal.module.css */
.wrap {
  opacity: 0;
  filter: blur(10px);
  transform: scale(1.04);
  transition:
    opacity  var(--duration, 800ms) ease                           var(--delay, 0ms),
    filter   var(--duration, 800ms) cubic-bezier(0.16, 1, 0.3, 1) var(--delay, 0ms),
    transform var(--duration, 800ms) cubic-bezier(0.16, 1, 0.3, 1) var(--delay, 0ms);
}

.revealed {
  opacity: 1;
  filter: blur(0);
  transform: scale(1);
}

@media (prefers-reduced-motion: reduce) {
  .wrap { transition: none; opacity: 1; filter: none; transform: none; }
}
```
