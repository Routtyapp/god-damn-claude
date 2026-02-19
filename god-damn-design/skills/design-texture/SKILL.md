---
name: design-texture
description: CSS/SVG 기반 시각적 질감(Texture) 구현 가이드. Glassmorphism, Grainy Gradients, Metallic 등 고급 표면 효과를 React 컴포넌트로 구현할 때 사용한다. backdrop-filter, SVG feTurbulence, mix-blend-mode, conic-gradient 등 질감 관련 CSS 기법이 필요할 때 이 스킬을 참조한다.
---

# Design Texture

CSS와 SVG 필터를 활용한 시각적 질감(Texture) 구현 스킬이다.

## 사용 시점

- 유리 효과(Glassmorphism) 카드/버튼 구현 시
- 노이즈 질감이 있는 그라데이션 배경 구현 시
- 메탈릭/크롬 느낌의 표면 효과 구현 시

## 핵심 기법 요약

| 질감 | 핵심 CSS/SVG | 용도 |
|------|-------------|------|
| Glassmorphism | `backdrop-filter: blur()` + `rgba` 배경 | 카드, 버튼, 오버레이 |
| Grainy Gradient | SVG `feTurbulence` + `mix-blend-mode` | 히어로 배경, 섹션 배경 |
| Metallic | `linear-gradient` 다단계 | 버튼, 배지, 장식 요소 |

## 공통 규칙

- 질감 레이어는 `pointer-events: none`으로 인터랙션 차단
- SVG 필터는 화면에 숨기되 DOM에 존재해야 한다 (`position: absolute; width: 0; height: 0`)
- 성능 주의: `backdrop-filter`, `mix-blend-mode`는 GPU 가속 대상이므로 과도한 중첩 금지

## 상세 참조

- **유리 효과**: [Glassmorphism.md](Glassmorphism.md) — 블러 카드, 3D 글래스 버튼, 재사용 컴포넌트
- **노이즈 질감**: [grainy-gradients.md](grainy-gradients.md) — SVG 노이즈 생성, 그라데이션 합성
- **메탈릭 효과**: [metalic.md](metalic.md) — 다단계 그라데이션 메탈 버튼
