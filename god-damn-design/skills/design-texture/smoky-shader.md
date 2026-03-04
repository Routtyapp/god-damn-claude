# Smoky Shader

WebGL Fragment Shader를 활용한 유체 연기 질감 효과다. `fragment-canvas` 라이브러리로 GLSL 셰이더를 Canvas에 렌더링하며, FBM(Fractional Brownian Motion) 알고리즘으로 유기적인 연기·불꽃 흐름을 시뮬레이션한다.

## 핵심 기법

- **FBM 노이즈**: 여러 주파수의 노이즈를 누적합산해 자연스러운 유체 움직임 생성
- **도메인 워핑(Domain Warping)**: `q → r → f` 3단계 FBM 합성으로 복잡한 소용돌이 표현
- **Canvas 레이어**: 버튼 뒤에 `-z-[1]`로 배치, hover 시 scale/rotate 트랜지션

## 의존성

```bash
npm install fragment-canvas
```

## 1. 기본 HTML 구조 (Tailwind)

```html
<button class="group relative rounded-[14px] text-base font-medium overflow-hidden">
  <!-- WebGL Canvas 배경 레이어 -->
  <div class="absolute -z-[1] -inset-4">
    <canvas
      class="absolute top-0 left-0 w-full h-full
             group-hover:scale-[1.2] group-hover:rotate-[10deg]
             transition-all duration-300"
      id="canvas"
    ></canvas>
  </div>
  <!-- 반투명 내용 레이어 -->
  <div class="m-[2px] px-6 py-2.5 rounded-[12px] bg-black/50
              group-hover:bg-black/25 transition-bg duration-300">
    <div class="text-white/80 group-hover:text-white
                group-hover:scale-[1.05] transition-all duration-300">
      Smoky Button
    </div>
  </div>
</button>
```

## 2. GLSL Fragment Shader

```glsl
precision mediump float;

uniform vec2 iResolution;
uniform float iTime;

/* 위치 기반 의사난수 */
float random(vec2 pos) {
    return fract(sin(dot(pos, vec2(12.9898, 78.233))) * 43758.5453);
}

/* Bilinear 보간 노이즈 */
float noise(vec2 pos) {
    vec2 i = floor(pos);
    vec2 f = fract(pos);
    float a = random(i + vec2(0.0, 0.0));
    float b = random(i + vec2(1.0, 0.0));
    float c = random(i + vec2(0.0, 1.0));
    float d = random(i + vec2(1.0, 1.0));
    vec2 u = f * f * (3.0 - 2.0 * f);  /* smoothstep */
    return mix(a, b, u.x) + (c - a) * u.y * (1.0 - u.x) + (d - b) * u.x * u.y;
}

/* Fractional Brownian Motion - 8 옥타브 */
float fbm(vec2 pos) {
    float v = 0.0;
    float a = 0.5;
    vec2 shift = vec2(20.0);
    mat2 rot = mat2(cos(0.5), sin(0.5), -sin(0.5), cos(0.5));
    for (int i = 0; i < 8; i++) {
        v += a * noise(pos);
        pos = rot * pos * 2.0 + shift;
        a *= 0.5;
    }
    return v;
}

void main(void) {
    vec2 uv = (gl_FragCoord.xy - 0.5 * iResolution.xy) / iResolution.y;
    uv *= 0.5;

    /* 1단계: q - 시간 기반 1차 워핑 */
    vec2 q = vec2(
        fbm(uv + 0.20 * iTime),
        fbm(uv + vec2(5.0, 1.0))
    );

    /* 2단계: r - q로 워핑된 2차 FBM */
    vec2 r = vec2(
        fbm(uv + 3.0 * q + vec2(1.2, 3.2) + 0.2 * iTime),
        fbm(uv + 3.0 * q + vec2(8.8, 2.8) + 0.2 * iTime)
    );

    /* 3단계: f - 최종 스칼라 값 */
    float f = fbm(uv + r);

    /* 색상 합성: 검정 → 파랑 → 마젠타 → 밝은 크림 */
    vec3 color = mix(
        vec3(0.0, 0.0, 0.0),
        vec3(0.0, 0.0, 1.0),
        clamp(f * f * 6.0, 0.0, 5.0)
    );
    color = mix(color, vec3(1.0, 0.0, 1.0), clamp(length(q) * length(q), 0.0, 1.0));
    color = mix(color, vec3(1.0, 1.0, 0.8), clamp(length(r.x), 0.0, 0.1));

    /* 어두운 베이스에 FBM 누적 밝기 적용 */
    color = vec3(0.2, 0.0, 0.15) + (f*f*f + 0.6*f*f + 0.6*f) * color;

    gl_FragColor = vec4(color, 1.0);
}
```

## 3. React 컴포넌트

```tsx
import { useEffect, useRef } from 'react';

const fragmentShader = `
  precision mediump float;
  uniform vec2 iResolution;
  uniform float iTime;

  float random(vec2 pos) {
    return fract(sin(dot(pos, vec2(12.9898, 78.233))) * 43758.5453);
  }
  float noise(vec2 pos) {
    vec2 i = floor(pos); vec2 f = fract(pos);
    float a = random(i), b = random(i + vec2(1,0));
    float c = random(i + vec2(0,1)), d = random(i + vec2(1,1));
    vec2 u = f * f * (3.0 - 2.0 * f);
    return mix(a,b,u.x) + (c-a)*u.y*(1.0-u.x) + (d-b)*u.x*u.y;
  }
  float fbm(vec2 p) {
    float v=0.0, a=0.5;
    mat2 rot = mat2(cos(0.5),sin(0.5),-sin(0.5),cos(0.5));
    for(int i=0;i<8;i++){v+=a*noise(p);p=rot*p*2.0+vec2(20.0);a*=0.5;}
    return v;
  }
  void main(void) {
    vec2 uv = (gl_FragCoord.xy - 0.5*iResolution.xy) / iResolution.y * 0.5;
    vec2 q = vec2(fbm(uv+0.2*iTime), fbm(uv+vec2(5,1)));
    vec2 r = vec2(
      fbm(uv+3.0*q+vec2(1.2,3.2)+0.2*iTime),
      fbm(uv+3.0*q+vec2(8.8,2.8)+0.2*iTime)
    );
    float f = fbm(uv+r);
    vec3 c = mix(vec3(0),vec3(0,0,1),clamp(f*f*6.0,0.0,5.0));
    c = mix(c,vec3(1,0,1),clamp(dot(q,q),0.0,1.0));
    c = mix(c,vec3(1,1,0.8),clamp(length(r.x),0.0,0.1));
    c = vec3(0.2,0.0,0.15)+(f*f*f+0.6*f*f+0.6*f)*c;
    gl_FragColor = vec4(c,1.0);
  }
`;

interface SmokyButtonProps {
  label?: string;
  onClick?: () => void;
}

export function SmokyButton({ label = 'Smoky Button', onClick }: SmokyButtonProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    import('fragment-canvas').then(({ default: FragmentCanvas }) => {
      const fc = new FragmentCanvas(canvas, { fragmentShader });

      const resize = () => {
        canvas.width = canvas.parentElement!.clientWidth * devicePixelRatio;
        canvas.height = canvas.parentElement!.clientHeight * devicePixelRatio;
      };
      resize();
      window.addEventListener('resize', resize);
      return () => window.removeEventListener('resize', resize);
    });
  }, []);

  return (
    <button
      onClick={onClick}
      className="group relative rounded-[14px] text-base font-medium overflow-hidden"
    >
      <div className="absolute -z-[1] -inset-4">
        <canvas
          ref={canvasRef}
          className="absolute top-0 left-0 w-full h-full
                     group-hover:scale-[1.2] group-hover:rotate-[10deg]
                     transition-all duration-300"
        />
      </div>
      <div className="m-[2px] px-6 py-2.5 rounded-[12px] bg-black/50
                      group-hover:bg-black/25 transition-all duration-300">
        <span className="text-white/80 group-hover:text-white
                         group-hover:scale-[1.05] transition-all duration-300 inline-block">
          {label}
        </span>
      </div>
    </button>
  );
}
```

## 4. 색상 커스터마이징

셰이더의 `main()` 색상 합성부만 교체하면 다른 팔레트를 만들 수 있다.

| 테마 | 베이스 색 | 1차 혼합 | 2차 혼합 |
|------|-----------|----------|----------|
| 기본 (보라/마젠타) | `vec3(0.2, 0.0, 0.15)` | `vec3(0,0,1)` | `vec3(1,0,1)` |
| 불꽃 (주황/빨강) | `vec3(0.2, 0.05, 0.0)` | `vec3(1,0.3,0)` | `vec3(1,0.8,0)` |
| 심해 (청록/파랑) | `vec3(0.0, 0.05, 0.2)` | `vec3(0,0.5,1)` | `vec3(0,1,0.8)` |
| 숲 (초록/황록) | `vec3(0.0, 0.1, 0.0)` | `vec3(0,0.8,0.2)` | `vec3(0.6,1,0)` |

## 5. 주의사항

- `fragment-canvas`는 ESM 전용 (`import`/`esm.sh` 방식)
- Canvas 크기는 `devicePixelRatio` 곱해서 고해상도 디스플레이 대응
- GPU 렌더링이므로 다수 인스턴스 동시 사용 시 성능 저하 가능
- `pointer-events: none`을 Canvas에 적용해 클릭 이벤트가 버튼에 전달되도록 할 것
