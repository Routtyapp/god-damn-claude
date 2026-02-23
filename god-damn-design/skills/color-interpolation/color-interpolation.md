# CSS Color Interpolation

두 색상 지점 사이의 중간 색상을 **어떤 색상 공간에서, 어떤 경로로** 계산할지 결정하는 기능이다.

---

## 1. 색상 공간 (Color Space)

### 직사각형 (Rectangular) — X/Y/Z 축 기반

```
srgb, srgb-linear, display-p3, a98-rgb,
prophoto-rgb, rec2020, lab, oklab, xyz, xyz-d50, xyz-d65
```

- 각 축이 독립적으로 보간됨
- Hue 방향 지정 불가

### 극좌표 (Polar) — Hue 각도 기반

```
hsl, hwb, lch, oklch
```

- Lightness / Chroma / Hue 구조
- **Hue 보간 방향 지정 가능** (shorter, longer, increasing, decreasing)
- 색상이 원통형으로 배열되어 있어 경로에 따라 결과가 달라짐

### 권장 색상 공간

| 용도 | 권장 공간 | 이유 |
|------|----------|------|
| 일반 그라디언트 | `oklch` | 지각적으로 균일, 선명 |
| 단순 색상 혼합 | `oklab` | 균일한 명도 유지 |
| 무지개/hue 순환 | `oklch` + `longer hue` | 전체 hue 휠 활용 |
| 레거시 호환 | `srgb` | 기존 동작과 동일 |

---

## 2. 구문

```css
in <색상공간> [<hue 보간 방향>]
```

```css
/* 직사각형 공간 — hue 방향 없음 */
linear-gradient(in oklch, red, blue)

/* 극좌표 공간 — hue 방향 추가 가능 */
linear-gradient(in oklch longer hue, red, blue)
```

---

## 3. Hue 보간 방향 (극좌표 공간 전용)

색조(Hue)는 0°~360° 원형이므로, 두 색상 사이를 짧은 쪽으로 갈지 긴 쪽으로 갈지 선택할 수 있다.

| 키워드 | 방향 | 결과 |
|--------|------|------|
| `shorter` **(기본값)** | 두 hue 사이 짧은 호 | 예측 가능한 직접 전환 |
| `longer` | 두 hue 사이 긴 호 | 반대쪽을 돌아 무지개 효과 |
| `increasing` | 항상 시계방향 (0°→360°) | 명시적 방향 고정 |
| `decreasing` | 항상 반시계방향 (360°→0°) | 명시적 방향 고정 |

```
        120° (녹색)
         |
180° ────┼──── 0°/360° (빨강)
         |
        240° (파랑)

빨강(0°) → 파랑(240°)
  shorter : 0° → 240° (시계방향 240° 이동)
  longer  : 0° → 360° → 240° (반시계방향 120° 이동, 녹색 통과)
```

---

## 4. linear-gradient()

```css
/* ❌ 기본 sRGB — 중간에 탁한 회색빛 */
background: linear-gradient(to right, red, blue);

/* ✅ oklch shorter (기본) — 선명한 직접 전환 */
background: linear-gradient(in oklch, red, blue);

/* 🌈 oklch longer hue — 무지개 전체를 통과 */
background: linear-gradient(in oklch longer hue, red, blue);

/* 방향 고정 */
background: linear-gradient(in oklch increasing hue, magenta, cyan);
background: linear-gradient(in oklch decreasing hue, magenta, cyan);
```

### 다중 색상 그라디언트

```css
/* oklch로 hue를 균일하게 분배 */
background: linear-gradient(
  in oklch,
  oklch(70% 0.3 0deg),
  oklch(70% 0.3 120deg),
  oklch(70% 0.3 240deg)
);
```

### Conic 그라디언트 (무지개 색상환)

```css
background: conic-gradient(
  from 0deg,
  oklch(70% 0.3 0deg),
  oklch(70% 0.3 120deg),
  oklch(70% 0.3 240deg),
  oklch(70% 0.3 360deg)
);
```

---

## 5. color-mix()

```css
/* 기본 구문 */
color-mix(in <색상공간>, <색상1> <비율>, <색상2> <비율>)

/* 비율 합계가 100%가 아니면 나머지는 투명으로 처리 */
```

```css
/* shorter hue (기본) — 직접 혼합 */
background-color: color-mix(
  in oklch,
  rgb(255 0 0) 50%,
  lch(60% 40% 220deg) 50%
);

/* longer hue — 반대 방향으로 돌아 혼합 */
background-color: color-mix(
  in oklch longer hue,
  rgb(255 0 0) 50%,
  lch(60% 40% 220deg) 50%
);
```

### 비율 조절

```css
/* 색상1을 30%만 사용 */
color-mix(in oklch, red 30%, blue)

/* 동적 비율 (CSS 변수 활용) */
color-mix(in oklch, var(--color-a) var(--ratio), var(--color-b))
```

### 실용 패턴 — 색상 톤 생성

```css
:root {
  --brand: oklch(60% 0.2 250deg);

  /* color-mix로 밝기/어두움 변형 */
  --brand-light: color-mix(in oklch, var(--brand) 60%, white);
  --brand-dark:  color-mix(in oklch, var(--brand) 60%, black);
  --brand-muted: color-mix(in oklch, var(--brand) 40%, gray);
}
```

---

## 6. Animation / Transition

```css
/* oklch 공간에서 색상 전환 — 중간값도 선명하게 유지 */
@keyframes bg-shift {
  from { background-color: oklch(30% 0.3 20deg);  }  /* 진한 핑크 */
  to   { background-color: oklch(70% 0.3 200deg); }  /* 청록색 */
}

.element {
  animation: bg-shift 2s ease-in-out infinite alternate;
}
```

```css
/* transition도 동일 — 브라우저가 중간 색상을 oklch 공간에서 계산 */
.button {
  background-color: oklch(55% 0.2 250deg);
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: oklch(45% 0.2 250deg);
}
```

---

## 7. sRGB vs oklch 비교

```css
/* sRGB — 두 색상을 RGB 채널(R, G, B)별로 선형 보간
   → 중간에서 채도가 떨어져 탁해짐 */
background: linear-gradient(to right, hsl(0 100% 50%), hsl(240 100% 50%));

/* oklch — 지각적으로 균일한 공간에서 보간
   → 중간 색상도 원래 채도 유지, 선명함 */
background: linear-gradient(in oklch, hsl(0 100% 50%), hsl(240 100% 50%));
```

| 비교 항목 | sRGB | oklch |
|----------|------|-------|
| 중간 색상 | 탁한 회색/보라 | 선명하게 유지 |
| 명도 균일성 | 불균일 | 지각적으로 균일 |
| Hue 방향 제어 | 불가 | shorter/longer 선택 |
| 무지개 효과 | 어려움 | `longer hue` 한 줄 |

---

## 8. React 활용 패턴

### CSS 변수 + color-mix 테마

```tsx
// 브랜드 색상 하나로 전체 팔레트 자동 생성
const theme = {
  '--brand': 'oklch(60% 0.2 250deg)',
  '--brand-50':  'color-mix(in oklch, var(--brand) 10%, white)',
  '--brand-100': 'color-mix(in oklch, var(--brand) 20%, white)',
  '--brand-200': 'color-mix(in oklch, var(--brand) 35%, white)',
  '--brand-700': 'color-mix(in oklch, var(--brand) 70%, black)',
  '--brand-900': 'color-mix(in oklch, var(--brand) 90%, black)',
} as React.CSSProperties;

<div style={theme}>...</div>
```

### 동적 그라디언트

```tsx
interface GradientProps {
  from: string;
  to: string;
  colorSpace?: string;
  hue?: 'shorter' | 'longer' | 'increasing' | 'decreasing';
  direction?: string;
}

const Gradient: React.FC<GradientProps> = ({
  from,
  to,
  colorSpace = 'oklch',
  hue = 'shorter',
  direction = 'to right',
}) => {
  const isPolar = ['hsl', 'hwb', 'lch', 'oklch'].includes(colorSpace);
  const interpolation = isPolar
    ? `in ${colorSpace} ${hue} hue`
    : `in ${colorSpace}`;

  return (
    <div
      style={{
        background: `linear-gradient(${interpolation} ${direction}, ${from}, ${to})`,
      }}
    />
  );
};

// 사용
<Gradient from="red" to="blue" colorSpace="oklch" hue="longer" />
```
