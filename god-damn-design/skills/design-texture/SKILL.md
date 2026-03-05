---
name: design-texture
description: CSS/SVG/WebGL 기반 시각적 질감(Texture) 구현 가이드. Glassmorphism, Grainy Gradients, Metallic, Smoky Shader 등 고급 표면 효과를 React 컴포넌트로 구현할 때 사용한다. backdrop-filter, SVG feTurbulence, mix-blend-mode, conic-gradient, GLSL Fragment Shader 등 질감 관련 기법이 필요할 때 이 스킬을 참조한다.
---

# Design Texture

CSS와 SVG 필터를 활용한 시각적 질감(Texture) 구현 스킬이다.

## 사용 시점

- 유리 효과(Glassmorphism) 카드/버튼 구현 시
- 노이즈 질감이 있는 그라데이션 배경 구현 시
- 메탈릭/크롬 느낌의 표면 효과 구현 시
- 빛이 물체를 통과하는 물리 조명을 시뮬레이션할 때
- GLSL 셰이더로 연기·유체·불꽃 같은 유기적 애니메이션 배경 구현 시

## 핵심 기법 요약

| 질감 | 핵심 CSS/SVG | 용도 |
|------|-------------|------|
| Glassmorphism | `backdrop-filter: blur()` + `rgba` 배경 | 카드, 버튼, 오버레이 |
| Grainy Gradient | SVG `feTurbulence` + alpha matrix + `mix-blend-mode` | 히어로 배경, 섹션 배경 |
| Horizon Lines | SVG `clipPath` 스캔라인 + `linearGradient` + 원형 빛 | 레트로/신스웨이브 배경 |
| Metallic | `linear-gradient` 다단계 | 버튼, 배지, 장식 요소 |
| Physical Light | `rotateX` + `linear-gradient` + `blur` + `:has()` | 프리미엄 카드, 조명 시뮬레이션 |
| Smoky Shader | GLSL FBM 노이즈 + `fragment-canvas` WebGL | 연기/불꽃 버튼, 라이브 배경 |

## 공통 규칙

- 질감 레이어는 `pointer-events: none`으로 인터랙션 차단
- SVG 필터는 화면에 숨기되 DOM에 존재해야 한다 (`position: absolute; width: 0; height: 0`)
- 성능 주의: `backdrop-filter`, `mix-blend-mode`는 GPU 가속 대상이므로 과도한 중첩 금지

## 상세 참조

- **유리 효과**: [Glassmorphism.md](Glassmorphism.md) — 블러 카드, 3D 글래스 버튼, 재사용 컴포넌트
- **노이즈 질감**: [grainy-gradients.md](grainy-gradients.md) — SVG 노이즈 생성, 4단계 필터 파이프라인, 그라데이션 합성
- **호라이즌 라인**: [horizon-lines.md](horizon-lines.md) — clipPath 스캔라인 마스크, 좌우/상하 원형 빛, 레트로 배경
- **메탈릭 효과**: [metallic.md](metallic.md) — 다단계 그라데이션 메탈 버튼
- **물리 조명**: [physical-light.md](physical-light.md) — slit 빛 투과, 3D 빛 확산, `:has()` 조명 토글
- **연기 셰이더**: [smoky-shader.md](smoky-shader.md) — GLSL FBM 도메인 워핑, WebGL 유체 배경, 색상 팔레트 커스터마이징
