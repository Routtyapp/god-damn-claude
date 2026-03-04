# Card Flip

## 구조 원칙

```
.scene        → perspective 설정, 크기 고정
  .card       → transform-style: preserve-3d, transition: rotateY
    .front    → 앞면, backface-visibility: hidden
    .back     → 뒷면, backface-visibility: hidden, 초기 rotateY(180deg)
```

## 1. CSS 전용 (hover trigger)

```tsx
// FlipCard.tsx
import styles from './FlipCard.module.css';
import { ReactNode } from 'react';

interface FlipCardProps {
  front: ReactNode;
  back: ReactNode;
  width?: number | string;
  height?: number | string;
  perspective?: number;
  className?: string;
}

export function FlipCard({
  front, back,
  width = 280, height = 380,
  perspective = 800,
  className,
}: FlipCardProps) {
  return (
    <div
      className={[styles.scene, className].filter(Boolean).join(' ')}
      style={{ width, height, perspective }}
    >
      <div className={styles.card}>
        <div className={styles.face}>{front}</div>
        <div className={[styles.face, styles.back].join(' ')}>{back}</div>
      </div>
    </div>
  );
}
```

```css
/* FlipCard.module.css */
.scene {
  /* perspective는 인라인으로 주입 */
  cursor: pointer;
}

.card {
  width: 100%; height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 600ms cubic-bezier(0.4, 0, 0.2, 1);
  will-change: transform;
}

/* hover OR :focus-within으로 flip */
.scene:hover .card,
.scene:focus-within .card {
  transform: rotateY(180deg);
}

.face {
  position: absolute;
  inset: 0;
  border-radius: 16px;
  backface-visibility: hidden;
  /* 렌더링 버그 방지 — Webkit */
  -webkit-backface-visibility: hidden;
  overflow: hidden;
}

.back {
  transform: rotateY(180deg);
}

@media (prefers-reduced-motion: reduce) {
  .card { transition: none; }
}
```

---

## 2. Click trigger (제어 컴포넌트)

```tsx
// ClickFlipCard.tsx
import { useState, KeyboardEvent } from 'react';
import styles from './FlipCard.module.css';
import { ReactNode } from 'react';

interface ClickFlipCardProps {
  front: ReactNode;
  back: ReactNode;
  width?: number | string;
  height?: number | string;
  frontLabel?: string;   // aria-label
  backLabel?: string;
}

export function ClickFlipCard({
  front, back, width = 280, height = 380,
  frontLabel = '카드 앞면. 클릭하여 뒷면 보기',
  backLabel = '카드 뒷면. 클릭하여 앞면 보기',
}: ClickFlipCardProps) {
  const [flipped, setFlipped] = useState(false);

  function handleKeyDown(e: KeyboardEvent) {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      setFlipped(f => !f);
    }
  }

  return (
    <div
      role="button"
      tabIndex={0}
      aria-label={flipped ? backLabel : frontLabel}
      aria-pressed={flipped}
      style={{ width, height, perspective: 800, cursor: 'pointer' }}
      onClick={() => setFlipped(f => !f)}
      onKeyDown={handleKeyDown}
    >
      <div
        className={styles.card}
        style={{ transform: flipped ? 'rotateY(180deg)' : 'none' }}
      >
        <div className={styles.face}>{front}</div>
        <div className={[styles.face, styles.back].join(' ')}>{back}</div>
      </div>
    </div>
  );
}
```

---

## 주의사항

- `perspective`는 반드시 **부모**에 설정. 자신에게 설정하면 효과 없음
- 뒷면 콘텐츠는 `rotateY(180deg)` — 텍스트가 거울처럼 뒤집혀 보이는 문제 없음 (이중 반전)
- `overflow: hidden`을 `.face`에 설정 — `.scene`에 걸면 3D 클리핑 버그 발생
- Safari에서 `-webkit-backface-visibility` 필요
- `will-change: transform` — 애니메이션 진행 중에만 유효, 전체 요소에 걸지 않음
