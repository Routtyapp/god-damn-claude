# Grainy Gradients

SVG 필터 파이프라인으로 그라디언트 위에 선명한 입자감(grain)을 생성하는 기법이다.
기본 노이즈에 대비 증폭 + 알파 임계값을 적용해 흐릿함 없이 또렷한 grain을 만든다.

---

## 핵심 원리

### 필터 파이프라인 (4단계)

```
feTurbulence
  → feColorMatrix(saturate 0)   : 노이즈 채도 제거 → 흑백
  → feComponentTransfer(slope 3): RGB 대비 3배 증폭
  → feColorMatrix(alpha matrix) : 알파 임계값으로 grain 이진화
  → mix-blend-mode: soft-light  : 그라디언트와 자연스럽게 합성
```

### 알파 임계값 행렬의 원리

```xml
<feColorMatrix type="matrix" values="
  1 0 0 0  0
  0 1 0 0  0
  0 0 1 0  0
  0 0 0 A -B" />
```

- R/G/B 채널은 그대로 통과
- **알파 채널만 변환**: `new_alpha = A * old_alpha - B`
- A가 클수록, B 절댓값이 클수록 → grain 경계가 더 선명하게 이진화됨
- B ≈ A - 8 관계 유지 (B가 너무 작으면 grain이 뭉개짐)

| A | B | 효과 |
|---|---|------|
| 15 | 7 | 부드러운 grain, 입자 크고 흐릿 |
| 17 | 9 | 균형잡힌 기본값 |
| 19 | 11 | 선명한 grain |
| 22 | 14 | 매우 날카로운 grain |

---

## 파라미터 레퍼런스

| 파라미터 | 역할 | 범위 | 기본값 |
|---------|------|------|--------|
| `baseFrequency` | 입자 크기 (높을수록 고운 grain) | 0.5 ~ 1.3 | 1.03 |
| `numOctaves` | 노이즈 디테일 레이어 수 | 2 ~ 4 | 2 |
| `seed` | 노이즈 패턴 시드 | 임의 정수 | 2 |
| `slope` (feComponentTransfer) | RGB 대비 증폭 배율 | 2 ~ 4 | 3 |
| A (alpha matrix) | grain 이진화 강도 | 15 ~ 25 | 17 |
| B (alpha matrix) | grain 임계값 오프셋 | A - 8 권장 | A - 8 |
| `opacity` (noise rect) | grain 전체 불투명도 | 0.3 ~ 1.0 | 0.67 |
| `mix-blend-mode` | 합성 방식 | soft-light | soft-light |

> `baseFrequency` 0.5~0.8 → 굵은 입자 (빈티지/인쇄물 느낌)
> `baseFrequency` 1.0~1.3 → 고운 입자 (필름/모래 느낌)

---

## 그라디언트 패턴

### Type A — Radial (방사형)

```svg
<radialGradient id="grain-gradient" r="1">
  <stop offset="0%"   stop-color="hsl(0, 22%, 79%)" />
  <stop offset="50%"  stop-color="hsl(227, 27%, 63%)" />
  <stop offset="100%" stop-color="hsla(290, 87%, 47%, 1)" />
</radialGradient>
```

중앙에서 바깥으로 색상이 퍼지는 구조. 3개 stop으로 풍부한 색감 연출.

```svg
<!-- 레이어 순서 -->
<rect fill="url(#grain-gradient)" />
<rect fill="transparent" filter="url(#grain-filter)" opacity="0.45" style="mix-blend-mode: soft-light" />
```

---

### Type B — Dual Linear (이중 선형, 권장)

두 개의 linear gradient를 반대 방향으로 배치해 베이스 색상 위에 색상 번짐 효과를 만든다.

```svg
<!-- gradient2: 한쪽 끝에서 투명하게 -->
<linearGradient gradientTransform="rotate(-309, 0.5, 0.5)" id="grain-g2">
  <stop offset="0%"   stop-color="hsla(290, 76%, 32%, 1)" stop-opacity="1" />
  <stop offset="100%" stop-color="rgba(255,255,255,0)"    stop-opacity="0" />
</linearGradient>

<!-- gradient3: 반대쪽 끝에서 투명하게 (각도 = -gradient2 각도) -->
<linearGradient gradientTransform="rotate(309, 0.5, 0.5)" id="grain-g3">
  <stop offset="0%"   stop-color="hsl(89, 60%, 60%)"   stop-opacity="1" />
  <stop offset="100%" stop-color="rgba(255,255,255,0)"  stop-opacity="0" />
</linearGradient>
```

```svg
<!-- 레이어 순서 -->
<rect fill="hsl(base-color)" />           <!-- 베이스 -->
<rect fill="url(#grain-g3)" />            <!-- 한쪽 색 -->
<rect fill="url(#grain-g2)" />            <!-- 반대쪽 색 -->
<rect fill="transparent" filter="url(#grain-filter)" opacity="1.0" style="mix-blend-mode: soft-light" />
```

> 두 그라디언트의 `rotate` 각도는 항상 부호만 반대 (예: -309 / +309)

---

## SVG 전체 필터 블록

```svg
<filter id="grain-filter" x="-20%" y="-20%" width="140%" height="140%"
        filterUnits="objectBoundingBox" primitiveUnits="userSpaceOnUse"
        color-interpolation-filters="sRGB">

  <!-- 1. 노이즈 생성 -->
  <feTurbulence
    type="fractalNoise"
    baseFrequency="1.03"
    numOctaves="2"
    seed="2"
    stitchTiles="stitch"
    result="turbulence" />

  <!-- 2. 흑백 변환 -->
  <feColorMatrix
    type="saturate" values="0"
    in="turbulence" result="colormatrix" />

  <!-- 3. RGB 대비 증폭 -->
  <feComponentTransfer in="colormatrix" result="componentTransfer">
    <feFuncR type="linear" slope="3" />
    <feFuncG type="linear" slope="3" />
    <feFuncB type="linear" slope="3" />
  </feComponentTransfer>

  <!-- 4. 알파 임계값 → grain 이진화 -->
  <feColorMatrix
    type="matrix"
    in="componentTransfer"
    values="1 0 0 0  0
            0 1 0 0  0
            0 0 1 0  0
            0 0 0 17 -9" />
</filter>
```

---

## React 컴포넌트

### SVG 인라인 방식 (권장)

```tsx
interface GrainyGradientProps {
  width?: number | string;
  height?: number | string;
  baseColor: string;         // 베이스 배경색 (hsl/hex)
  gradientColors: [string, string];  // [끝1 색, 끝2 색]
  gradientAngle?: number;    // 기본 309
  baseFrequency?: number;    // 기본 1.03
  alphaA?: number;           // grain 강도 (기본 17)
  alphaB?: number;           // grain 오프셋 (기본 9)
  noiseOpacity?: number;     // 기본 0.67
}

export const GrainyGradient: React.FC<GrainyGradientProps> = ({
  width = '100%',
  height = '100%',
  baseColor,
  gradientColors,
  gradientAngle = 309,
  baseFrequency = 1.03,
  alphaA = 17,
  alphaB = 9,
  noiseOpacity = 0.67,
}) => {
  const id = React.useId().replace(/:/g, '');

  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 700 700"
      width={width}
      height={height}
      preserveAspectRatio="xMidYMid slice"
    >
      <defs>
        <linearGradient
          id={`${id}-g2`}
          gradientTransform={`rotate(-${gradientAngle}, 0.5, 0.5)`}
          x1="50%" y1="0%" x2="50%" y2="100%"
        >
          <stop offset="0%"   stopColor={gradientColors[0]} stopOpacity={1} />
          <stop offset="100%" stopColor="rgba(255,255,255,0)" stopOpacity={0} />
        </linearGradient>

        <linearGradient
          id={`${id}-g3`}
          gradientTransform={`rotate(${gradientAngle}, 0.5, 0.5)`}
          x1="50%" y1="0%" x2="50%" y2="100%"
        >
          <stop offset="0%"   stopColor={gradientColors[1]} stopOpacity={1} />
          <stop offset="100%" stopColor="rgba(255,255,255,0)" stopOpacity={0} />
        </linearGradient>

        <filter
          id={`${id}-filter`}
          x="-20%" y="-20%" width="140%" height="140%"
          filterUnits="objectBoundingBox"
          primitiveUnits="userSpaceOnUse"
          colorInterpolationFilters="sRGB"
        >
          <feTurbulence
            type="fractalNoise"
            baseFrequency={baseFrequency}
            numOctaves={2}
            seed={2}
            stitchTiles="stitch"
            result="turbulence"
          />
          <feColorMatrix type="saturate" values="0" in="turbulence" result="colormatrix" />
          <feComponentTransfer in="colormatrix" result="componentTransfer">
            <feFuncR type="linear" slope={3} />
            <feFuncG type="linear" slope={3} />
            <feFuncB type="linear" slope={3} />
          </feComponentTransfer>
          <feColorMatrix
            type="matrix"
            in="componentTransfer"
            values={`1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 ${alphaA} -${alphaB}`}
          />
        </filter>
      </defs>

      <g>
        <rect width="100%" height="100%" fill={baseColor} />
        <rect width="100%" height="100%" fill={`url(#${id}-g3)`} />
        <rect width="100%" height="100%" fill={`url(#${id}-g2)`} />
        <rect
          width="100%" height="100%"
          fill="transparent"
          filter={`url(#${id}-filter)`}
          opacity={noiseOpacity}
          style={{ mixBlendMode: 'soft-light' }}
        />
      </g>
    </svg>
  );
};
```

### CSS 배경으로 사용하기

SVG를 data URI로 인라인 삽입해 `background-image`로 사용할 수도 있다.
단, React 컴포넌트 방식이 파라미터 제어가 용이해 더 권장된다.

```css
.grainy-bg {
  background-image: url("data:image/svg+xml,...");
  background-size: cover;
}
```

---

## 사용 레시피

### 부드러운 grain (필름 감도 낮은 느낌)
```
baseFrequency: 0.75, alphaA: 15, alphaB: 7, noiseOpacity: 0.45
```

### 기본 grain (균형)
```
baseFrequency: 1.03, alphaA: 17, alphaB: 9, noiseOpacity: 0.67
```

### 선명한 grain (인쇄물/리소그래피)
```
baseFrequency: 1.13, alphaA: 22, alphaB: 14, noiseOpacity: 0.67
```

### 극도로 고운 grain (노이즈 최소화)
```
baseFrequency: 1.3, alphaA: 19, alphaB: 11, noiseOpacity: 0.3
```

---

## 주의사항

- `filter` 영역을 `-20% ~ 140%`로 확장하지 않으면 **가장자리에 grain이 끊기는** 아티팩트 발생
- `stitchTiles="stitch"`는 타일 경계를 매끄럽게 이어줌 — 반드시 포함
- 같은 페이지에 여러 인스턴스 사용 시 `id` 충돌 방지 필수 → `useId()` 활용
- `mix-blend-mode: soft-light`는 밝은 배경에 더 잘 맞음; 어두운 배경은 `overlay`도 시도
- SVG 인라인 방식은 CSS `background`보다 파라미터 제어가 쉽고 ID 충돌도 관리 가능
