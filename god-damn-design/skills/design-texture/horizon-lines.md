# Horizon Lines

SVG clipPath로 점진적으로 좁아지는 수평 스캔라인 띠를 만들고, 선형 그라디언트 + 원형 빛을 합성해 레트로 호라이즌/신스웨이브 배경을 구현하는 기법이다.

---

## 핵심 원리

### 구조

```
SVG
  defs
    linearGradient #grad   (위 → 아래, 어두운 → 밝은)
    linearGradient #grad2  (밝은 → 어두운, #grad의 역방향)
    clipPath               (수평 스캔라인 마스크)
  g
    rect   (전체 배경, #grad)    ← clipPath 적용
    circle (원형 빛 1, #grad2)   ← clipPath 적용
    circle (원형 빛 2, #grad2)   ← clipPath 적용
```

### ClipPath 스캔라인 알고리즘

n개의 수평 rect를 등간격(총높이 / n)으로 배치하되, 각 rect의 높이를 위쪽 n → 아래쪽 1로 감소시킨다.

```
총 height = H, 스트라이프 수 = n (기본 47)
step = H / n (≈ 17px for H=800, n=47)

stripe[i]:
  y      = round(i * step)         (i = 0 ~ n-1)
  height = n - i                   (n → 1로 감소)
```

**시각적 효과:**
- 위쪽: rect 높이 ≈ 47px, step ≈ 17px → 큰 겹침 → 거의 채워진 면
- 아래쪽: rect 높이 ≈ 1~5px, step ≈ 17px → 큰 공백 → 가느다란 선만 남음

결과: 상단은 연속적인 그라디언트, 하단으로 갈수록 스캔라인이 촘촘해지다가 사라지는 호라이즌 효과.

---

## 그라디언트 구성

두 그라디언트는 항상 명도 기준으로 역방향 쌍을 이룬다.

```svg
<!-- grad: 위쪽(어두운/채도 낮은) → 아래쪽(밝은/채도 높은) -->
<linearGradient id="grad" x1="50%" y1="0%" x2="50%" y2="100%">
  <stop offset="25%"  stop-color="hsl(H, S%, L_dark%)" />
  <stop offset="100%" stop-color="hsl(H, S%, L_light%)" />
</linearGradient>

<!-- grad2: 역방향 (원형 빛에 사용) -->
<linearGradient id="grad2" x1="50%" y1="0%" x2="50%" y2="100%">
  <stop offset="0%"  stop-color="hsl(H, S%, L_light%)" />
  <stop offset="75%" stop-color="hsl(H, S%, L_dark%)" />
</linearGradient>
```

> `grad`의 첫 stop이 25%에서 시작하는 이유: 0~25% 구간은 단색으로 유지해 상단에 "하늘" 영역을 만든다.

---

## 변형 패턴

### Variant A — Side Circles (좌우 빛)

원이 좌우 가장자리에 위치. 빛이 화면 양 측면에서 들어오는 느낌.

```svg
<!-- 레이어 순서 (모두 동일한 clipPath 적용) -->
<rect width="800" height="800" fill="url(#grad)" clip-path="url(#cp)" />
<circle r="400" cx="800" cy="400" fill="url(#grad2)" clip-path="url(#cp)" />  <!-- 오른쪽 -->
<circle r="400" cx="0"   cy="400" fill="url(#grad2)" clip-path="url(#cp)" />  <!-- 왼쪽 -->
```

```
┌────────────────────────────┐
│         (grad)             │  ← 상단: 어두운 단색
│ ●grad2          grad2●     │  ← 중앙: 원형 빛 양쪽
│    ═══════════════════     │  ← 하단: 스캔라인 + 빛
└────────────────────────────┘
```

### Variant B — Top/Bottom Circles (상하 빛)

원이 상하 가운데에 위치. 빛이 위아래에서 들어와 가운데로 퍼지는 느낌.

```svg
<!-- 원 위치만 다름. grad2 대신 grad 사용 가능 -->
<rect width="800" height="800" fill="url(#grad)" clip-path="url(#cp)" />
<circle r="400" cx="400" cy="0"   fill="url(#grad)" clip-path="url(#cp)" />  <!-- 상단 -->
<circle r="400" cx="400" cy="800" fill="url(#grad)" clip-path="url(#cp)" />  <!-- 하단 -->
```

```
┌────────────────────────────┐
│           ●                │  ← 상단 원
│    ═══════════════════     │  ← 스캔라인
│           ●                │  ← 하단 원
└────────────────────────────┘
```

---

## 파라미터

| 파라미터 | 역할 | 기본값 |
|---------|------|--------|
| `size` | viewBox 크기 (정사각형) | `800` |
| `numStripes` | 스캔라인 수 — 많을수록 촘촘 | `47` |
| `colorDark` | 어두운 색 (HSL) | 필수 |
| `colorLight` | 밝은 색 (HSL) | 필수 |
| `circleVariant` | `'side'` \| `'top-bottom'` | `'side'` |
| `opacity` | SVG 전체 불투명도 | `0.75` |

> **색상 선택:** `colorDark`와 `colorLight`는 같은 Hue에서 lightness만 20~30 차이 나게 설정할 때 가장 자연스럽다. 보색(complementary) 조합은 psychedelic한 느낌을 낸다.

---

## React 컴포넌트

```tsx
interface HorizonLinesProps {
  size?: number;
  numStripes?: number;
  colorDark: string;    // e.g. "hsl(265, 55%, 30%)"
  colorLight: string;   // e.g. "hsl(265, 55%, 60%)"
  circleVariant?: 'side' | 'top-bottom';
  opacity?: number;
  className?: string;
  style?: React.CSSProperties;
}

export const HorizonLines: React.FC<HorizonLinesProps> = ({
  size = 800,
  numStripes = 47,
  colorDark,
  colorLight,
  circleVariant = 'side',
  opacity = 0.75,
  className,
  style,
}) => {
  const id = React.useId().replace(/:/g, '');
  const step = size / numStripes;

  // 스캔라인 clipPath 생성
  const stripes = Array.from({ length: numStripes }, (_, i) => ({
    y: Math.round(i * step),
    height: numStripes - i,
  }));

  const clipId = `${id}-cp`;
  const gradId = `${id}-grad`;
  const grad2Id = `${id}-grad2`;

  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox={`0 0 ${size} ${size}`}
      width="100%"
      height="100%"
      opacity={opacity}
      className={className}
      style={style}
      preserveAspectRatio="xMidYMid slice"
    >
      <defs>
        {/* 메인 그라디언트: 어두운 → 밝은 */}
        <linearGradient id={gradId} x1="50%" y1="0%" x2="50%" y2="100%">
          <stop offset="25%"  stopColor={colorDark}  stopOpacity={1} />
          <stop offset="100%" stopColor={colorLight} stopOpacity={1} />
        </linearGradient>

        {/* 역방향 그라디언트 (Side 원에 사용) */}
        <linearGradient id={grad2Id} x1="50%" y1="0%" x2="50%" y2="100%">
          <stop offset="0%"  stopColor={colorLight} stopOpacity={1} />
          <stop offset="75%" stopColor={colorDark}  stopOpacity={1} />
        </linearGradient>

        {/* 스캔라인 마스크 */}
        <clipPath id={clipId}>
          {stripes.map((s, i) => (
            <rect key={i} x={0} y={s.y} width={size} height={s.height} />
          ))}
        </clipPath>
      </defs>

      <g>
        {/* 배경 rect */}
        <rect
          width={size} height={size}
          fill={`url(#${gradId})`}
          clipPath={`url(#${clipId})`}
        />

        {/* 원형 빛 */}
        {circleVariant === 'side' ? (
          <>
            <circle r={size / 2} cx={size} cy={size / 2}
              fill={`url(#${grad2Id})`} clipPath={`url(#${clipId})`} />
            <circle r={size / 2} cx={0}    cy={size / 2}
              fill={`url(#${grad2Id})`} clipPath={`url(#${clipId})`} />
          </>
        ) : (
          <>
            <circle r={size / 2} cx={size / 2} cy={0}
              fill={`url(#${gradId})`} clipPath={`url(#${clipId})`} />
            <circle r={size / 2} cx={size / 2} cy={size}
              fill={`url(#${gradId})`} clipPath={`url(#${clipId})`} />
          </>
        )}
      </g>
    </svg>
  );
};
```

---

## 사용 레시피

### 딥 퍼플 (Variant A — Side Circles)
```tsx
<HorizonLines
  colorDark="hsl(265, 55%, 30%)"
  colorLight="hsl(265, 55%, 60%)"
  circleVariant="side"
  opacity={0.75}
/>
```

### 코럴-민트 (Variant B — Top/Bottom)
```tsx
<HorizonLines
  colorDark="hsl(1, 100%, 67%)"
  colorLight="hsl(167, 52%, 78%)"
  circleVariant="top-bottom"
  opacity={0.75}
/>
```

### 미드나잇 블루 (Variant A, 촘촘한 라인)
```tsx
<HorizonLines
  colorDark="hsl(220, 80%, 15%)"
  colorLight="hsl(200, 70%, 50%)"
  numStripes={60}
  circleVariant="side"
  opacity={0.85}
/>
```

---

## 배경으로 사용하기

```tsx
// 섹션 배경에 절대 포지셔닝으로 깔기
<section style={{ position: 'relative', overflow: 'hidden' }}>
  <div style={{
    position: 'absolute', inset: 0,
    zIndex: 0, pointerEvents: 'none',
  }}>
    <HorizonLines
      colorDark="hsl(265, 55%, 30%)"
      colorLight="hsl(265, 55%, 60%)"
    />
  </div>
  <div style={{ position: 'relative', zIndex: 1 }}>
    {/* 콘텐츠 */}
  </div>
</section>
```

---

## 주의사항

- `numStripes`가 너무 많으면 (>80) 성능 저하 가능 — 47~60이 최적
- `circleVariant="top-bottom"`은 위아래 원이 동일한 `#grad`를 쓰므로 색이 단조로울 수 있음 — 그라디언트 stop offset을 조정해 변화를 주는 것을 권장
- `preserveAspectRatio="xMidYMid slice"`로 부모 크기에 맞게 꽉 채워지도록 설정
- 다른 텍스처(Grainy Gradient)와 레이어 합성 시 `opacity` 낮추고 `mix-blend-mode: screen` 시도
