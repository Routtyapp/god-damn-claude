# Blob Pattern

CSS `clip-path: shape()` / `border-radius` / SVG `path` 기반의 유기적 blob 형태 구현 컴포넌트이다.

---

## 1. CSS clip-path shape() Blob ✦ 권장

`clip-path: shape()`를 사용한 가장 정밀한 순수 CSS blob이다.
SVG 없이 cubic bezier 곡선을 직접 CSS에서 정의한다.

> **브라우저 지원**: Chrome 116+, Edge 116+, Safari 18.2+, Firefox 136+ (2024~2025 기준 모던 브라우저 대응)

### 구조

```css
.blob {
  /* shape() 함수가 생성하는 자연 비율 유지 */
  aspect-ratio: 0.925;

  clip-path: shape(
    from 91.52% 26.2%,
    curve to 93.52% 78.28% with 101.76% 42.67% / 103.09% 63.87%,
    curve to 44.11% 99.97% with 83.95% 92.76% / 63.47% 100.58%,
    curve to 1.45%  78.42% with 24.74% 99.42% / 6.42%  90.43%,
    curve to 14.06% 35.46% with -3.45% 66.41% / 4.93%  51.38%,
    curve to 47.59% 0.33%  with 23.18% 19.54% / 33.13% 2.8%,
    curve to 91.52% 26.2%  with 62.14% -2.14% / 81.28% 9.66%
  );
}
```

### shape() 문법

```
from x% y%                              ← 시작점 (SVG path의 M)
curve to x% y% with cx1% cy1%/cx2% cy2% ← cubic bezier (SVG path의 C)
```

- `from` — 시작 좌표 (% 기준)
- `curve to` — 도착 좌표
- `with cx1/cy1 / cx2/cy2` — 두 개의 제어점

### React 컴포넌트

```tsx
interface ShapeBlobProps {
  width?: number | string;
  color?: string;
  shape?: string;
  aspectRatio?: number;
  className?: string;
  children?: React.ReactNode;
}

// shape() 문자열만 교체하면 형태 변경 가능
const DEFAULT_SHAPE = `
  from 91.52% 26.2%,
  curve to 93.52% 78.28% with 101.76% 42.67% / 103.09% 63.87%,
  curve to 44.11% 99.97% with 83.95%  92.76% / 63.47%  100.58%,
  curve to 1.45%  78.42% with 24.74%  99.42% / 6.42%   90.43%,
  curve to 14.06% 35.46% with -3.45%  66.41% / 4.93%   51.38%,
  curve to 47.59% 0.33%  with 23.18%  19.54% / 33.13%  2.8%,
  curve to 91.52% 26.2%  with 62.14%  -2.14% / 81.28%  9.66%
`;

const ShapeBlob: React.FC<ShapeBlobProps> = ({
  width = 300,
  color = '#a78bfa',
  shape = DEFAULT_SHAPE,
  aspectRatio = 0.925,
  className,
  children,
}) => {
  return (
    <div
      className={className}
      style={{
        width,
        aspectRatio,
        backgroundColor: color,
        clipPath: `shape(${shape})`,
      }}
    >
      {children}
    </div>
  );
};
```

### 사용 예시

```tsx
{/* 기본 blob */}
<ShapeBlob width={300} color="#a78bfa" />

{/* gradient blob */}
<ShapeBlob
  width={400}
  style={{ background: 'linear-gradient(135deg, #818cf8, #c084fc)' }}
/>

{/* 이미지 마스크 */}
<div
  style={{
    width: 300,
    aspectRatio: 0.925,
    clipPath: `shape(${DEFAULT_SHAPE})`,
    overflow: 'hidden',
  }}
>
  <img src="/photo.jpg" style={{ width: '100%', height: '100%', objectFit: 'cover' }} />
</div>
```

### shape() Morph 애니메이션

shape() blob 간 보간이 가능해 자연스러운 morph 애니메이션을 만들 수 있다.
**단, 두 shape()의 point 개수가 동일해야 보간 작동.**

```css
@keyframes shape-morph {
  0%, 100% {
    clip-path: shape(
      from 91.52% 26.2%,
      curve to 93.52% 78.28% with 101.76% 42.67% / 103.09% 63.87%,
      curve to 44.11% 99.97% with 83.95%  92.76% / 63.47%  100.58%,
      curve to 1.45%  78.42% with 24.74%  99.42% / 6.42%   90.43%,
      curve to 14.06% 35.46% with -3.45%  66.41% / 4.93%   51.38%,
      curve to 47.59% 0.33%  with 23.18%  19.54% / 33.13%  2.8%,
      curve to 91.52% 26.2%  with 62.14%  -2.14% / 81.28%  9.66%
    );
  }
  50% {
    clip-path: shape(
      from 80% 20%,
      curve to 95% 60%  with 100% 30%  / 105% 50%,
      curve to 55% 95%  with 90%  80%  / 70%   100%,
      curve to 10% 70%  with 30%  95%  / 5%    85%,
      curve to 5%  30%  with 0%   55%  / -5%   40%,
      curve to 40% 5%   with 15%  15%  / 25%   0%,
      curve to 80% 20%  with 55%  10%  / 70%   5%
    );
  }
}

.animated-shape-blob {
  animation: shape-morph 8s ease-in-out infinite;
}
```

### shape() 생성 도구

직접 값을 계산하기 어렵기 때문에 아래 도구를 활용한다.
- [CSS Blob Generator (Josh W Comeau 등)](https://www.blobmaker.app)
- 생성된 SVG path를 shape() 문법으로 변환하거나, 전용 생성기 사용

---

## 2. CSS border-radius Blob (간단한 유기체 형태)

`border-radius`에 8개 값을 지정해 불규칙한 유기체 형태를 만든다.

### React 컴포넌트

```tsx
interface CssBlobProps {
  width?: number | string;
  height?: number | string;
  color?: string;
  borderRadius?: string;
  className?: string;
  children?: React.ReactNode;
}

const CssBlob: React.FC<CssBlobProps> = ({
  width = 300,
  height = 300,
  color = '#a78bfa',
  borderRadius = '60% 40% 30% 70% / 60% 30% 70% 40%',
  className,
  children,
}) => {
  return (
    <div
      className={className}
      style={{
        width,
        height,
        backgroundColor: color,
        borderRadius,
      }}
    >
      {children}
    </div>
  );
};
```

### 사용 예시

```tsx
{/* 기본 blob */}
<CssBlob
  width={300}
  height={300}
  color="#a78bfa"
  borderRadius="60% 40% 30% 70% / 60% 30% 70% 40%"
/>

{/* 납작한 가로형 blob */}
<CssBlob
  width={400}
  height={200}
  color="#34d399"
  borderRadius="69% 31% 50% 50% / 31% 38% 62% 69%"
/>

{/* 세로형 blob */}
<CssBlob
  width={200}
  height={350}
  color="#f472b6"
  borderRadius="42% 58% 70% 30% / 45% 45% 55% 55%"
/>
```

### border-radius 8값 구조

```
좌상-수평  우상-수평  우하-수평  좌하-수평
    /
좌상-수직  우상-수직  우하-수직  좌하-수직
```

### 자주 쓰는 preset

| 형태 | border-radius 값 |
|------|-----------------|
| 기본 유기체 | `60% 40% 30% 70% / 60% 30% 70% 40%` |
| 둥근 삼각형 | `70% 30% 30% 70% / 70% 70% 30% 30%` |
| 납작 타원 | `69% 31% 50% 50% / 31% 38% 62% 69%` |
| 물방울 | `50% 50% 50% 50% / 60% 60% 40% 40%` |
| 비대칭 | `42% 58% 70% 30% / 45% 45% 55% 55%` |

---

## 3. SVG Path Blob (이미지 클립 / 레거시 대응)

SVG `d` 속성의 cubic bezier 곡선으로 더 정교한 blob을 만든다.

### React 컴포넌트

```tsx
interface SvgBlobProps {
  size?: number;
  color?: string;
  path?: string;
  className?: string;
  children?: React.ReactNode;
}

// 중심(150,150) 기준 300x300 viewBox 기본값
const DEFAULT_PATH =
  'M150,30 C220,20 280,80 290,150 C300,220 240,280 165,285 C90,290 30,240 25,165 C20,90 80,40 150,30 Z';

const SvgBlob: React.FC<SvgBlobProps> = ({
  size = 300,
  color = '#818cf8',
  path = DEFAULT_PATH,
  className,
  children,
}) => {
  const id = React.useId();

  return (
    <div className={className} style={{ width: size, height: size, position: 'relative' }}>
      <svg
        viewBox="0 0 300 300"
        width={size}
        height={size}
        style={{ position: 'absolute', inset: 0 }}
      >
        <defs>
          <clipPath id={`blob-clip-${id}`}>
            <path d={path} />
          </clipPath>
        </defs>
        <path d={path} fill={color} />
      </svg>
      {children && (
        <div style={{ position: 'relative', zIndex: 1, width: '100%', height: '100%' }}>
          {children}
        </div>
      )}
    </div>
  );
};
```

### 사용 예시

```tsx
{/* 기본 SVG blob */}
<SvgBlob size={300} color="#818cf8" />

{/* 이미지 클립 blob */}
<SvgBlobClip size={300} imageSrc="/photo.jpg" />

{/* 커스텀 path */}
<SvgBlob
  size={400}
  color="#fb923c"
  path="M145,20 C230,15 295,75 295,160 C295,245 230,295 145,295 C60,295 10,235 10,155 C10,75 60,25 145,20 Z"
/>
```

### 이미지 클립 blob

```tsx
interface SvgBlobClipProps {
  size?: number;
  imageSrc: string;
  path?: string;
}

const SvgBlobClip: React.FC<SvgBlobClipProps> = ({
  size = 300,
  imageSrc,
  path = DEFAULT_PATH,
}) => {
  const id = React.useId();

  return (
    <svg viewBox="0 0 300 300" width={size} height={size}>
      <defs>
        <clipPath id={`blob-clip-${id}`}>
          <path d={path} />
        </clipPath>
      </defs>
      <image
        href={imageSrc}
        x="0"
        y="0"
        width="300"
        height="300"
        preserveAspectRatio="xMidYMid slice"
        clipPath={`url(#blob-clip-${id})`}
      />
    </svg>
  );
};
```

### 주요 SVG path preset

```
// 부드러운 원형
M150,30 C220,20 280,80 290,150 C300,220 240,280 165,285 C90,290 30,240 25,165 C20,90 80,40 150,30 Z

// 넓적한 타원형
M40,120 C50,50 130,20 200,40 C270,60 300,130 280,200 C260,265 180,290 110,270 C40,250 30,190 40,120 Z

// 비대칭 유기체
M160,25 C240,10 300,85 295,165 C290,245 215,300 140,290 C65,280 10,210 20,135 C30,60 80,40 160,25 Z
```

---

## 4. Animation Blob (border-radius morph)

### CSS border-radius Morph

```tsx
interface AnimatedBlobProps {
  size?: number | string;
  color?: string;
  duration?: number;
  className?: string;
}

const AnimatedBlob: React.FC<AnimatedBlobProps> = ({
  size = 300,
  color = '#c084fc',
  duration = 8,
  className,
}) => {
  return (
    <>
      <style>{`
        @keyframes blob-morph {
          0%   { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
          25%  { border-radius: 30% 70% 70% 30% / 50% 60% 40% 50%; }
          50%  { border-radius: 50% 50% 20% 80% / 25% 80% 20% 75%; }
          75%  { border-radius: 70% 30% 50% 50% / 40% 40% 60% 60%; }
          100% { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
        }

        @media (prefers-reduced-motion: reduce) {
          .animated-blob { animation: none !important; }
        }
      `}</style>
      <div
        className={`animated-blob ${className ?? ''}`}
        style={{
          width: size,
          height: size,
          backgroundColor: color,
          animation: `blob-morph ${duration}s ease-in-out infinite`,
        }}
      />
    </>
  );
};
```

### 사용 예시

```tsx
{/* 기본 morph */}
<AnimatedBlob size={300} color="#c084fc" duration={8} />

{/* 느린 배경용 blob */}
<AnimatedBlob size={500} color="#6ee7b7" duration={14} />

{/* 빠른 강조용 blob */}
<AnimatedBlob size={120} color="#fb7185" duration={4} />
```

### Gradient + Morph 조합

```tsx
const GradientAnimatedBlob: React.FC<{ size?: number; duration?: number }> = ({
  size = 300,
  duration = 8,
}) => {
  const id = React.useId();

  return (
    <>
      <style>{`
        @keyframes blob-morph {
          0%   { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
          25%  { border-radius: 30% 70% 70% 30% / 50% 60% 40% 50%; }
          50%  { border-radius: 50% 50% 20% 80% / 25% 80% 20% 75%; }
          75%  { border-radius: 70% 30% 50% 50% / 40% 40% 60% 60%; }
          100% { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
        }
      `}</style>
      <div
        style={{
          width: size,
          height: size,
          background: 'linear-gradient(135deg, #818cf8, #c084fc, #fb7185)',
          animation: `blob-morph ${duration}s ease-in-out infinite`,
        }}
      />
    </>
  );
};
```

---

## 5. 조절 속성 요약

| 속성 | 방식 | 역할 | 권장값 |
|------|------|------|--------|
| `clip-path: shape()` | shape() | 정밀 CSS 형태 제어 | 생성기 활용, point 수 고정 |
| `aspect-ratio` | shape() | 자연 비율 유지 | 생성기가 제공하는 값 그대로 사용 |
| `borderRadius` (8값) | border-radius | 간단한 유기체 형태 | 30~70% 범위에서 조절 |
| `path` | SVG | 정밀 형태 + 이미지 클립 | cubic bezier 곡선 |
| `duration` | animation | morph 속도 | 배경용 12~16s, 강조용 4~6s |
| `color` / `background` | 공통 | 색상 | 단색 또는 linear-gradient |
| `size` / `width` | 공통 | 크기 | 용도에 따라 자유롭게 |

---

## 6. 사용 시나리오

| 시나리오 | 권장 타입 |
|----------|-----------|
| 히어로 배경 장식 | Gradient + AnimatedBlob |
| 프로필 이미지 마스크 | SvgBlobClip |
| 섹션 분리 요소 | CssBlob (정적) |
| 로딩/주목 CTA 강조 | AnimatedBlob (빠른 duration) |
| 브랜드 일러스트 배경 | SvgBlob (커스텀 path) |
