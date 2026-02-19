# Grid Pattern

CSS background-image + 인라인 SVG를 활용한 격자 반복 패턴 컴포넌트이다.

## 1. React 컴포넌트

```tsx
import React from 'react';

interface PatternProps {
  gridSize?: number;      // 격자 한 칸의 크기 (기본 80)
  lineColor?: string;     // 격자 선 색상
  lineWidth?: number;     // 격자 선 두께
  patternColor?: string;  // 문양 색상
  patternType?: 'plus' | 'squircle'; // 문양 형태
  patternSize?: number;   // 문양의 크기
}

const DesignGridPattern: React.FC<PatternProps> = ({
  gridSize = 80,
  lineColor = '#5abae5',
  lineWidth = 3,
  patternColor = 'rgb(48, 17, 15)',
  patternType = 'plus',
  patternSize = 28,
}) => {
  const center = gridSize / 2;
  const halfSize = patternSize / 2;

  // 문양 디자인 선택 (SVG Path)
  const drawPattern = () => {
    if (patternType === 'plus') {
      return `<path d='M${center} ${center - halfSize} v${patternSize} M${center - halfSize} ${center} h${patternSize}'
                stroke='${patternColor}' stroke-width='${lineWidth * 2.5}' stroke-linecap='round'/>`;
    } else {
      return `<rect x='${center - halfSize}' y='${center - halfSize}'
                width='${patternSize}' height='${patternSize}' rx='${patternSize / 3}' fill='${patternColor}'/>`;
    }
  };

  // CSS 배경 스타일 계산
  const svgData = `url("data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' width='${gridSize}' height='${gridSize}'>${drawPattern()}</svg>")`;

  const halfLine = lineWidth / 2;
  const lineStart = center - halfLine;
  const lineEnd = center + halfLine;

  const backgroundStyle: React.CSSProperties = {
    width: '100%',
    height: '100%',
    backgroundColor: '#fdfbf7',
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

## 2. 사용 예시

```tsx
import DesignGridPattern from './DesignGridPattern';

function App() {
  return (
    <div style={{ width: '500px', height: '500px', borderRadius: '20px', overflow: 'hidden' }}>
      {/* 기본 스타일 */}
      <DesignGridPattern
        gridSize={80}
        lineColor="#5abae5"
        lineWidth={3}
        patternType="plus"
      />

      {/* 조밀하고 둥근 사각형 스타일 */}
      {/* <DesignGridPattern
        gridSize={40}
        lineColor="#ddd"
        lineWidth={1}
        patternType="squircle"
        patternSize={12}
        patternColor="#555"
      /> */}
    </div>
  );
}
```

## 3. 주요 조절 속성

| 속성 | 역할 | 효과 |
|------|------|------|
| `gridSize` | 격자 한 칸의 정방형 크기 | 클수록 듬성듬성 |
| `lineWidth` | 격자선 두께 | 1이면 섬세, 5+이면 팝아트 |
| `patternType` | 문양 형태 (`plus` / `squircle`) | 십자 또는 둥근 점 |
| `patternSize` | 문양 전체 크기 | 격자 대비 도형 비율 |
