# Depth Stack

`translateZ`로 레이어 간 깊이감을 주는 패턴.

## 1. Layered Card (isometric stack)

```tsx
// LayeredCard.tsx
import styles from './LayeredCard.module.css';
import { ReactNode } from 'react';

interface Layer {
  content?: ReactNode;
  background: string;
  zOffset?: number;    // translateZ 값 (px), 기본 -20px 간격
  xyOffset?: number;   // x/y 오프셋 (px)
  opacity?: number;
}

interface LayeredCardProps {
  layers: Layer[];       // index 0 = 최상단
  width?: number;
  height?: number;
  rotateX?: number;      // 기본 20
  rotateY?: number;      // 기본 -20
}

export function LayeredCard({
  layers,
  width = 240,
  height = 300,
  rotateX = 20,
  rotateY = -20,
}: LayeredCardProps) {
  return (
    <div
      className={styles.scene}
      style={{ width, height, '--rotate-x': `${rotateX}deg`, '--rotate-y': `${rotateY}deg` } as React.CSSProperties}
    >
      {[...layers].reverse().map((layer, i) => {
        const reverseIdx = layers.length - 1 - i;
        const zOffset = layer.zOffset ?? reverseIdx * -20;
        const xyOffset = layer.xyOffset ?? reverseIdx * 12;
        return (
          <div
            key={i}
            className={styles.layer}
            style={{
              background: layer.background,
              transform: `translateZ(${zOffset}px) translateX(${xyOffset}px) translateY(${xyOffset}px)`,
              opacity: layer.opacity ?? 1,
              zIndex: layers.length - reverseIdx,
            }}
          >
            {layer.content}
          </div>
        );
      })}
    </div>
  );
}
```

```css
/* LayeredCard.module.css */
.scene {
  position: relative;
  transform-style: preserve-3d;
  transform:
    rotateX(var(--rotate-x, 20deg))
    rotateY(var(--rotate-y, -20deg));
  transition: transform 400ms cubic-bezier(0.34, 1.56, 0.64, 1);
}

.scene:hover {
  transform:
    rotateX(calc(var(--rotate-x, 20deg) * 0.7))
    rotateY(calc(var(--rotate-y, -20deg) * 0.7));
}

.layer {
  position: absolute;
  inset: 0;
  border-radius: 16px;
  will-change: transform;
  transition: transform 400ms cubic-bezier(0.34, 1.56, 0.64, 1);
}

@media (prefers-reduced-motion: reduce) {
  .scene, .layer { transition: none; }
}
```

### 사용 예시

```tsx
<LayeredCard
  width={240} height={280}
  rotateX={15} rotateY={-20}
  layers={[
    {
      background: 'linear-gradient(135deg, #6366f1, #4f46e5)',
      content: (
        <div style={{ padding: 24, color: '#fff' }}>
          <h3>카드 제목</h3>
        </div>
      ),
    },
    { background: '#4f46e5', opacity: 0.7 },
    { background: '#3730a3', opacity: 0.4 },
  ]}
/>
```

---

## 2. Depth Text (텍스트 글자별 z-depth)

```css
/* 글자별로 translateZ를 달리해 입체 텍스트 효과 */
.depth-text {
  display: flex;
  transform-style: preserve-3d;
  perspective: 400px;
  gap: 2px;
}

.depth-text span {
  display: inline-block;
  transform: translateZ(calc(var(--depth, 0) * 1px));
  text-shadow:
    0 calc(var(--depth, 0) * 0.1px) calc(var(--depth, 0) * 0.5px) rgba(0,0,0,0.4);
  transition: transform 300ms cubic-bezier(0.34, 1.56, 0.64, 1);
}

.depth-text:hover span { --depth: 20; }
```

```tsx
// DepthText.tsx
export function DepthText({ text }: { text: string }) {
  return (
    <div className="depth-text" aria-label={text}>
      {text.split('').map((char, i) => (
        <span
          key={i}
          aria-hidden="true"
          style={{ '--depth': i % 2 === 0 ? 16 : 4 } as React.CSSProperties}
        >
          {char === ' ' ? '\u00A0' : char}
        </span>
      ))}
    </div>
  );
}
```

---

## 주의사항

- `transform-style: preserve-3d` 요소에 `overflow: hidden` 금지 — 3D 클리핑 버그
- `border-radius`와 `preserve-3d`의 공존: 자식 레이어에 `border-radius` 각각 설정
- 레이어 수는 3개 이하 권장 — 많을수록 compositing layer 부담 증가
- 실제 `z-index`와 CSS `translateZ`는 stacking context상 연관 없음. `z-index`를 별도로 관리
