# Cinema-like Background

SVG feSpecularLighting과 커서 추적을 활용한 시네마틱 배경 효과이다.

## 1. React 컴포넌트

```tsx
import React, { useEffect, useRef } from 'react';
import './DynamicBackground.css';

const DynamicBackground: React.FC = () => {
  const containerRef = useRef<HTMLDivElement>(null);
  const cursorPointRef = useRef<HTMLDivElement>(null);
  const cursorLightRef = useRef<HTMLDivElement>(null);
  const pointLightRef = useRef<SVGFEPointLightElement>(null);
  const turbulenceRef = useRef<SVGFETurbulenceElement>(null);

  // 마우스 좌표 및 부드러운 이동을 위한 변수 (리렌더링 방지를 위해 useRef로 관리)
  const mouse = useRef({
    x: 0,
    y: 0,
    smoothX: 0,
    smoothY: 0,
    noise: 0
  });

  useEffect(() => {
    const onMouseMove = (e: MouseEvent) => {
      mouse.current.x = e.clientX;
      mouse.current.y = e.clientY;
    };

    window.addEventListener('mousemove', onMouseMove);

    let animationFrameId: number;

    const tick = () => {
      const m = mouse.current;

      // 1. 부드러운 마우스 이동 계산 (Lerp)
      m.smoothX += (m.x - m.smoothX) * 0.1;
      m.smoothY += (m.y - m.smoothY) * 0.1;

      // 2. SVG 빛 위치 업데이트
      if (pointLightRef.current) {
        pointLightRef.current.setAttribute('x', m.smoothX.toString());
        pointLightRef.current.setAttribute('y', m.smoothY.toString());
      }

      // 3. 노이즈 애니메이션 업데이트
      if (turbulenceRef.current) {
        m.noise += 0.5;
        turbulenceRef.current.setAttribute('seed', Math.round(m.noise).toString());
      }

      // 4. 커서 요소 이동
      if (cursorPointRef.current) {
        cursorPointRef.current.style.transform = `translate3d(${m.x}px, ${m.y}px, 0)`;
      }
      if (cursorLightRef.current) {
        cursorLightRef.current.style.transform = `translate3d(${m.smoothX}px, ${m.smoothY}px, 0)`;
      }

      animationFrameId = requestAnimationFrame(tick);
    };

    tick();

    return () => {
      window.removeEventListener('mousemove', onMouseMove);
      cancelAnimationFrame(animationFrameId);
    };
  }, []);

  return (
    <>
      <div className="container" ref={containerRef}>
        <h1 className="title">
          This is <br />
          just SVG
        </h1>
        <div className="subtitle">How cool is that?</div>
      </div>

      <div className="cursor">
        <div className="cursor__point" ref={cursorPointRef}></div>
        <div className="cursor__light" ref={cursorLightRef}></div>
      </div>

      <svg xmlns="http://www.w3.org/2000/svg" className="svg-filters">
        <filter id="light">
          <feSpecularLighting result="specOut" surfaceScale="1" specularConstant={1.2} specularExponent={10} lightingColor="#999">
            <fePointLight id="point-light" ref={pointLightRef} x="50" y="75" z="100" />
          </feSpecularLighting>
        </filter>

        <filter id="noise">
          <feTurbulence id="turbulence" ref={turbulenceRef} type="fractalNoise" baseFrequency="0.999 0.999" seed="1" numOctaves={10} />
          <feColorMatrix type="saturate" values="0" />
          <feComponentTransfer result="noise">
            <feFuncA type="linear" slope="1" />
          </feComponentTransfer>
          <feBlend in="noise" in2="SourceGraphic" mode="multiply" />
        </filter>
      </svg>
    </>
  );
};

export default DynamicBackground;
```

## 2. CSS

```css
body {
  margin: 0;
  background: black;
  cursor: none;
  overflow: hidden;
}

.container {
  display: flex;
  position: relative;
  width: 100%;
  height: 100vh;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  filter: url('#noise') url('#light');
  color: #fff;
  text-align: center;
  z-index: 1;
}

.title {
  margin: 0;
  font-size: 10vw;
  font-family: serif;
  text-transform: uppercase;
  line-height: 0.9;
}

.cursor__point,
.cursor__light {
  position: fixed;
  top: 0;
  left: 0;
  border-radius: 50%;
  pointer-events: none;
  z-index: 100;
  background: white;
}

.cursor__point {
  width: 6px;
  height: 6px;
  margin-left: -3px;
  margin-top: -3px;
}

.cursor__light {
  width: 100px;
  height: 100px;
  margin-left: -50px;
  margin-top: -50px;
  opacity: 0.15;
  filter: blur(20px);
}

.svg-filters {
  position: absolute;
  width: 0;
  height: 0;
  pointer-events: none;
}
```
