# Grainy Gradients

SVG 노이즈 필터를 생성하고, CSS 그라데이션 위에 덧씌워 모래알 같은 입자감을 만드는 기법이다.

## 핵심 원리

- **feTurbulence**: 수학적 알고리즘으로 무작위 노이즈(구름, 모래 질감)를 생성
- **feColorMatrix**: 노이즈의 색상을 조정하거나 채도를 낮춰 은은한 입자로 변환
- **mix-blend-mode**: 노이즈와 그라데이션을 `overlay` 또는 `multiply`로 합성

## 1. React 컴포넌트

```tsx
import React from 'react';
import './GrainyGradient.css';

const GrainyGradient: React.FC = () => {
  return (
    <div className="gradient-wrapper">
      {/* 1. 배경이 되는 그라데이션 레이어 */}
      <div className="gradient-background"></div>

      {/* 2. 질감을 만들어주는 노이즈 레이어 (필터 적용) */}
      <div className="grainy-overlay"></div>

      {/* 3. SVG 필터 정의 (화면에는 보이지 않음) */}
      <svg xmlns="http://www.w3.org/2000/svg" className="svg-filter-container">
        <filter id="grainyNoise">
          {/* baseFrequency가 높을수록 입자가 고와진다 (0.5 ~ 0.9 추천) */}
          <feTurbulence
            type="fractalNoise"
            baseFrequency="0.65"
            numOctaves="3"
            stitchTiles="stitch"
          />
          {/* 노이즈의 대비를 조절하여 질감을 더 선명하게 만든다 */}
          <feColorMatrix type="saturate" values="0" />
        </filter>
      </svg>

      <div className="content">
        <h1>Grainy Gradients</h1>
        <p>SVG Filter + React Interaction</p>
      </div>
    </div>
  );
};

export default GrainyGradient;
```

## 2. CSS

```css
.gradient-wrapper {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
  background-color: #ff3c00;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* 다채로운 그라데이션 층 */
.gradient-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle at 50% 50%, #ffbb00 0%, #ff3c00 50%, #7a00ff 100%);
  z-index: 1;
}

/* 핵심: 노이즈 필터를 적용한 레이어 */
.grainy-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 2;
  filter: url('#grainyNoise');
  mix-blend-mode: overlay;
  opacity: 0.4;
  pointer-events: none;
}

.content {
  position: relative;
  z-index: 3;
  color: white;
  text-align: center;
  font-family: sans-serif;
}

.svg-filter-container {
  position: absolute;
  width: 0;
  height: 0;
}
```

## 3. 주요 파라미터

| 파라미터 | 역할 | 추천값 |
|---------|------|--------|
| `numOctaves` | 노이즈 디테일 수준 | 3~5 |
| `baseFrequency` | 입자 크기 (작으면 구름, 크면 고운 모래) | 0.5~0.9 |
| `mix-blend-mode` | 합성 방식 | `overlay` (기본), `multiply` (빈티지) |
| `opacity` | 노이즈 강도 | 0.3~0.5 |
