# Grid Pattern

CSS background-image + 인라인 SVG를 활용한 격자 반복 패턴 컴포넌트이다.
`renderPattern`으로 SVG 요소를 자유롭게 지정할 수 있다.

## 1. 타입 정의

```tsx
/** renderPattern 콜백에 전달되는 좌표 컨텍스트 */
interface PatternContext {
  center: number;      // 격자 한 칸의 중심 좌표 (= gridSize / 2)
  half: number;        // patternSize / 2
  size: number;        // patternSize 그대로
  color: string;       // patternColor
  strokeWidth: number; // lineWidth * 2.5 (선형 문양 기준)
}

interface PatternProps {
  gridSize?: number;        // 격자 한 칸의 크기 (기본 80)
  lineColor?: string;       // 격자 선 색상
  lineWidth?: number;       // 격자 선 두께
  patternColor?: string;    // 문양 색상 (renderPattern에서 ctx.color로 접근)
  patternSize?: number;     // 문양의 크기
  /**
   * SVG 요소 문자열을 반환하는 렌더 함수.
   * 지정하지 않으면 기본 프리셋(plus)이 사용된다.
   * 좌표 기준: (0,0) ~ (gridSize, gridSize), 중심은 ctx.center
   */
  renderPattern?: (ctx: PatternContext) => string;
}
```

## 2. React 컴포넌트

```tsx
import React from 'react';

// 기본 프리셋: 십자(plus)
const defaultRender = ({ center, half, size, color, strokeWidth }: PatternContext): string =>
  `<path d='M${center} ${center - half} v${size} M${center - half} ${center} h${size}'
     stroke='${color}' stroke-width='${strokeWidth}' stroke-linecap='round'/>`;

const DesignGridPattern: React.FC<PatternProps> = ({
  gridSize = 80,
  lineColor = '#5abae5',
  lineWidth = 3,
  patternColor = 'rgb(48, 17, 15)',
  patternSize = 28,
  renderPattern = defaultRender,
}) => {
  const center = gridSize / 2;
  const ctx: PatternContext = {
    center,
    half: patternSize / 2,
    size: patternSize,
    color: patternColor,
    strokeWidth: lineWidth * 2.5,
  };

  const svgInner = renderPattern(ctx);
  const svgData = `url("data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' width='${gridSize}' height='${gridSize}'>${svgInner}</svg>")`;

  const halfLine = lineWidth / 2;
  const lineStart = center - halfLine;
  const lineEnd = center + halfLine;

  const backgroundStyle: React.CSSProperties = {
    width: '100%',
    height: '100%',
    backgroundImage: `
      ${svgData},
      linear-gradient(to right, transparent ${lineStart}px, ${lineColor} ${lineStart}px, ${lineColor} ${lineEnd}px, transparent ${lineEnd}px),
      linear-gradient(to bottom, transparent ${lineStart}px, ${lineColor} ${lineStart}px, ${lineColor} ${lineEnd}px, transparent ${lineEnd}px)
    `,
    backgroundSize: `${gridSize}px ${gridSize}px`,
    backgroundRepeat: 'repeat',
    backgroundPosition: 'center center',
  };

  return <div style={backgroundStyle} />;
};

export default DesignGridPattern;
```

## 3. 내장 프리셋 모음

자주 쓰는 도형은 아래 프리셋을 `renderPattern`에 바로 넣을 수 있다.

```tsx
import type { PatternContext } from './DesignGridPattern';

export const Presets = {
  /** 십자(기본) */
  plus: ({ center, half, size, color, strokeWidth }: PatternContext) =>
    `<path d='M${center} ${center - half} v${size} M${center - half} ${center} h${size}'
       stroke='${color}' stroke-width='${strokeWidth}' stroke-linecap='round'/>`,

  /** 둥근 사각형 */
  squircle: ({ center, half, size, color }: PatternContext) =>
    `<rect x='${center - half}' y='${center - half}' width='${size}' height='${size}'
       rx='${size / 3}' fill='${color}'/>`,

  /** 원 */
  circle: ({ center, half, color }: PatternContext) =>
    `<circle cx='${center}' cy='${center}' r='${half}' fill='${color}'/>`,

  /** 다이아몬드 */
  diamond: ({ center, half, color }: PatternContext) =>
    `<polygon points='${center},${center - half} ${center + half},${center} ${center},${center + half} ${center - half},${center}'
       fill='${color}'/>`,

  /** X자 */
  cross: ({ center, half, color, strokeWidth }: PatternContext) =>
    `<path d='M${center - half} ${center - half} L${center + half} ${center + half} M${center + half} ${center - half} L${center - half} ${center + half}'
       stroke='${color}' stroke-width='${strokeWidth}' stroke-linecap='round'/>`,

  /** 삼각형 */
  triangle: ({ center, half, color }: PatternContext) =>
    `<polygon points='${center},${center - half} ${center + half},${center + half} ${center - half},${center + half}'
       fill='${color}'/>`,
};
```

## 4. 사용 예시

```tsx
import DesignGridPattern from './DesignGridPattern';
import { Presets } from './presets';

function App() {
  return (
    <div style={{ width: '500px', height: '500px', borderRadius: '20px', overflow: 'hidden' }}>

      {/* 프리셋 사용 */}
      <DesignGridPattern renderPattern={Presets.circle} patternColor="#e879f9" />

      {/* 인라인 커스텀 — 별 모양 */}
      <DesignGridPattern
        gridSize={100}
        patternSize={36}
        renderPattern={({ center, half, color }) =>
          `<polygon points='
            ${center},${center - half}
            ${center + half * 0.35},${center - half * 0.35}
            ${center + half},${center}
            ${center + half * 0.35},${center + half * 0.35}
            ${center},${center + half}
            ${center - half * 0.35},${center + half * 0.35}
            ${center - half},${center}
            ${center - half * 0.35},${center - half * 0.35}
           ' fill='${color}'/>`
        }
      />

      {/* 인라인 커스텀 — 텍스트/이모지 */}
      <DesignGridPattern
        gridSize={80}
        renderPattern={({ center }) =>
          `<text x='${center}' y='${center + 6}' text-anchor='middle'
             font-size='20' fill='%23666'>✦</text>`
        }
      />

    </div>
  );
}
```

> **SVG 인라인 인코딩 주의**: `data:image/svg+xml` URL 안에서는 `#`을 `%23`으로 인코딩해야 한다.
> `patternColor` prop으로 색상을 넘기면 컴포넌트가 `ctx.color`로 전달하므로 직접 인코딩 불필요.

## 5. 주요 조절 속성

| 속성 | 역할 | 효과 |
|------|------|------|
| `gridSize` | 격자 한 칸의 정방형 크기 | 클수록 듬성듬성 |
| `lineWidth` | 격자선 두께 | 1이면 섬세, 5+이면 팝아트 |
| `patternSize` | 문양 전체 크기 | 격자 대비 도형 비율 |
| `patternColor` | 문양 색상 | `ctx.color`로 전달됨 |
| `renderPattern` | SVG 요소 렌더 함수 | 프리셋 또는 완전 커스텀 |
