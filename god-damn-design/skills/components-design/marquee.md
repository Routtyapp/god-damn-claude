# Infinite Marquee

## 개요

콘텐츠(텍스트, 로고, 카드)를 수평으로 끊김 없이 무한 반복 스크롤하는 UI 패턴.
고객사 로고 나열, 슬로건 강조, 수상 내역 등 시각적 밀도와 리듬이 필요한 섹션에 사용한다.

---

## 구조 원리

콘텐츠를 **정확히 2회 복사**한 뒤 translateX(0) → translateX(-50%) 로 이동시키면
끝에 닿는 순간 처음과 이어져 무한 루프처럼 보인다.

```
[콘텐츠 복사본 A][콘텐츠 복사본 B]  ← 트랙 너비 = 콘텐츠 2×
         ↑ -50% 이동하면 B 시작점 = A 시작점과 동일
```

---

## 기본 구현 (순수 CSS)

```html
<div class="marquee">
  <div class="track">
    <div class="content">
      로고A &nbsp; 로고B &nbsp; 로고C &nbsp; 로고D &nbsp;
      로고A &nbsp; 로고B &nbsp; 로고C &nbsp; 로고D &nbsp;
    </div>
  </div>
</div>
```

```css
.marquee {
  position: relative;
  width: 100%;
  overflow: hidden;
}

.track {
  position: absolute;
  white-space: nowrap;
  will-change: transform;
  animation: marquee 20s linear infinite;
}

@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}

/* 접근성: 모션 축소 설정 시 정지 */
@media (prefers-reduced-motion: reduce) {
  .track { animation: none; }
}
```

---

## React 컴포넌트

```tsx
import { useRef } from 'react';

interface MarqueeProps {
  children: React.ReactNode;
  speed?: number;       // 초 단위, 낮을수록 빠름 (기본 20)
  gap?: string;         // 아이템 간격 (기본 '2rem')
  direction?: 'left' | 'right';
  pauseOnHover?: boolean;
}

export function Marquee({
  children,
  speed = 20,
  gap = '2rem',
  direction = 'left',
  pauseOnHover = true,
}: MarqueeProps) {
  return (
    <div
      className="marquee-root"
      style={{ '--gap': gap, '--speed': `${speed}s` } as React.CSSProperties}
    >
      <div
        className={[
          'marquee-track',
          direction === 'right' && 'marquee-track--reverse',
          pauseOnHover && 'marquee-track--pause-hover',
        ]
          .filter(Boolean)
          .join(' ')}
        aria-hidden="true"
      >
        {/* 2벌 복사 */}
        <div className="marquee-inner">{children}</div>
        <div className="marquee-inner" aria-hidden="true">{children}</div>
      </div>
    </div>
  );
}
```

```css
.marquee-root {
  overflow: hidden;
  width: 100%;
}

.marquee-track {
  display: flex;
  width: max-content;
  animation: marquee var(--speed, 20s) linear infinite;
}

.marquee-track--reverse {
  animation-direction: reverse;
}

.marquee-track--pause-hover:hover {
  animation-play-state: paused;
}

.marquee-inner {
  display: flex;
  align-items: center;
  gap: var(--gap, 2rem);
  padding-right: var(--gap, 2rem); /* 마지막 아이템과 복사본 사이 간격 유지 */
}

@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}

@media (prefers-reduced-motion: reduce) {
  .marquee-track { animation: none; }
}
```

### 사용 예시

```tsx
// 고객사 로고
<Marquee speed={30} gap="3rem">
  <img src="/logos/acme.svg" alt="Acme" height={32} />
  <img src="/logos/globex.svg" alt="Globex" height={32} />
  <img src="/logos/initech.svg" alt="Initech" height={32} />
</Marquee>

// 슬로건 텍스트 (큰 타이포)
<Marquee speed={40} gap="4rem" pauseOnHover={false}>
  <span>We ship fast</span>
  <span aria-hidden="true">✦</span>
  <span>Design matters</span>
  <span aria-hidden="true">✦</span>
</Marquee>

// 역방향 두 줄 (엇갈린 리듬)
<Marquee speed={25}>...</Marquee>
<Marquee speed={25} direction="right">...</Marquee>
```

---

## 엣지 케이스

| 문제 | 원인 | 해결 |
|------|------|------|
| 끊김(jump) 발생 | 두 복사본 사이 간격 불일치 | `padding-right`로 마지막 아이템 뒤 간격 보정 |
| 느린 화면에서 깜빡임 | `position: absolute` 사용 시 repaint | `display: flex` + `width: max-content` 사용 |
| 너무 빠르거나 느림 | 콘텐츠 길이에 무관한 고정 시간 | JS로 `trackWidth / viewportWidth * baseSpeed` 계산 후 CSS 변수 주입 |
| 로고 이미지 비율 깨짐 | height 미지정 | `height` 고정 + `width: auto` |

---

## 속도 동적 계산 (선택)

콘텐츠가 길수록 빠르게, 짧으면 느리게 — **픽셀/초 기준**으로 일정하게 유지할 때.

```tsx
const trackRef = useRef<HTMLDivElement>(null);

useEffect(() => {
  if (!trackRef.current) return;
  const PX_PER_SEC = 80; // 원하는 속도
  const trackW = trackRef.current.scrollWidth / 2; // 복사본 1벌 너비
  const duration = trackW / PX_PER_SEC;
  trackRef.current.style.setProperty('--speed', `${duration}s`);
}, []);
```

---

## 접근성

- 장식용 반복 콘텐츠는 `aria-hidden="true"` (두 번째 복사본)
- 실제 정보가 포함된 경우(로고 브랜드명) `alt` 텍스트 필수
- `prefers-reduced-motion: reduce` 시 `animation: none` 처리
- `pauseOnHover`는 UX 친화적이지만 필수 아님
