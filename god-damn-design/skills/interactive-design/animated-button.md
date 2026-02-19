# Animated Button

clip-path와 CSS 변수 도형을 활용한 셰이프 변환 애니메이션 버튼이다.

## 1. 벡터 도형 컴포넌트 (React)

```tsx
import React from 'react';

type PatternShape = 'star' | 'flower' | 'cylinder' | 'hexagon' | 'circle';

interface BlueBottleProps {
  gridSize?: number;
  lineColor?: string;
  lineWidth?: number;
  patternColor?: string;
  patternType?: PatternShape;
  patternSize?: number;
}

const UniversalPattern: React.FC<BlueBottleProps> = ({
  gridSize = 80,
  lineColor = '#5abae5',
  lineWidth = 2,
  patternColor = '#30110f',
  patternType = 'star',
  patternSize = 30,
}) => {
  const center = gridSize / 2;
  const s = patternSize / 2;

  // 5가지 벡터 도형 정의 루틴
  const getShape = () => {
    switch (patternType) {
      case 'star':
        return `<path d="M${center} ${center-s*1.2} L${center+s*0.4} ${center-s*0.4} L${center+s*1.2} ${center} L${center+s*0.4} ${center+s*0.4} L${center} ${center+s*1.2} L${center-s*0.4} ${center+s*0.4} L${center-s*1.2} ${center} L${center-s*0.4} ${center-s*0.4} Z" fill="${patternColor}" />`;
      case 'flower':
        return `<path d="M${center} ${center-s} C${center+s} ${center-s} ${center+s} ${center+s} ${center} ${center+s} C${center-s} ${center+s} ${center-s} ${center-s} ${center} ${center-s} M${center-s} ${center} C${center-s} ${center-s} ${center+s} ${center-s} ${center+s} ${center} C${center+s} ${center+s} ${center-s} ${center+s} ${center-s} ${center}" fill="${patternColor}" />`;
      case 'cylinder':
        return `<rect x="${center-s*0.7}" y="${center-s}" width="${s*1.4}" height="${s*2}" rx="${s*0.7}" fill="${patternColor}" />`;
      case 'hexagon':
        return `<path d="M${center} ${center-s} L${center+s*0.8} ${center-s*0.5} L${center+s*0.8} ${center+s*0.5} L${center} ${center+s} L${center-s*0.8} ${center+s*0.5} L${center-s*0.8} ${center-s*0.5} Z" fill="${patternColor}" stroke="${patternColor}" stroke-width="4" stroke-linejoin="round" />`;
      case 'circle':
        return `<circle cx="${center}" cy="${center}" r="${s*0.9}" fill="${patternColor}" />`;
    }
  };

  const svgData = `url("data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' width='${gridSize}' height='${gridSize}'>${getShape()}</svg>")`;

  const backgroundStyle: React.CSSProperties = {
    width: '100%',
    height: '100%',
    backgroundColor: '#fdfbf7',
    backgroundImage: `
      ${svgData},
      linear-gradient(to right, transparent ${center - lineWidth/2}px, ${lineColor} ${center - lineWidth/2}px, ${lineColor} ${center + lineWidth/2}px, transparent ${center + lineWidth/2}px),
      linear-gradient(to bottom, transparent ${center - lineWidth/2}px, ${lineColor} ${center - lineWidth/2}px, ${lineColor} ${center + lineWidth/2}px, transparent ${center + lineWidth/2}px)
    `,
    backgroundSize: `${gridSize}px ${gridSize}px`,
    backgroundPosition: 'center center',
  };

  return <div style={backgroundStyle} />;
};

export default UniversalPattern;
```

## 2. HTML 구조

```html
<div id="animated-button">
  <div></div>
</div>
```

## 3. CSS 변수 도형 정의

```css
:root {
  --circle: circle(50% at 50% 50%);
  --flower: transition-path(M 50 0 C 70 0 100 30 100 50 C 100 70 70 100 50 100 C 30 100 0 70 0 50 C 0 30 30 0 50 0);
  --hexagon: polygon(25% 0%, 75% 0%, 100% 50%, 75% 100%, 25% 100%, 0% 50%);
  --star: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%);
  --cylinder: inset(10% 20% 10% 20% round 50px);
}
```

## 4. CSS 스타일링

```css
#animated-button {
  width: 200px;
  aspect-ratio: 1;
  margin: 50px auto;
  position: relative;

  &::before {
    content: "";
    clip-path: var(--star);
    width: 100%;
    height: 100%;
    position: absolute;
    background-color: #494949;
    transition: 1s ease-in-out;
  }

  &:hover::before {
    transform: rotate(180deg);
    background-color: #FAFBFE;
  }
}

#animated-button div {
  width: 100%;
  height: 100%;
  clip-path: var(--flower);
  scale: 0;
  transition: 1s ease-in-out;

  &::after {
    content: "";
    background: linear-gradient(135deg, #217bfe, #078efb, #ac87eb, #217bfe);
    width: 400%;
    height: 400%;
    position: absolute;
  }
}

#animated-button:hover div {
  scale: 1;
  animation: shapeshift 5s ease-in-out infinite forwards;
}
```

## 5. Keyframes 애니메이션

```css
@keyframes shapeshift {
  0% {
    clip-path: var(--circle);
    rotate: 0turn;
  }
  25% {
    clip-path: var(--flower);
  }
  50% {
    clip-path: var(--cylinder);
  }
  75% {
    clip-path: var(--hexagon);
  }
  100% {
    clip-path: var(--circle);
    rotate: 1turn;
  }
}

@keyframes gradientMove {
  0% {
    translate: 0 0;
  }
  100% {
    translate: -75% -75%;
  }
}
```
