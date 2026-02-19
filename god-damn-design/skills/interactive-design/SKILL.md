---
name: interactive-design
description: CSS 애니메이션과 SVG 필터를 활용한 인터랙티브 디자인 패턴 가이드. clip-path 도형 변환, keyframes 애니메이션, SVG feSpecularLighting 조명 효과, 커서 추적 인터랙션 등 고급 시각 연출이 필요할 때 사용한다. hover 애니메이션, 마우스 반응형 배경, 시네마틱 UI 효과를 구현할 때 이 스킬을 참조한다.
---

# Interactive Design

CSS 애니메이션과 SVG 필터를 결합한 인터랙티브 시각 효과 스킬이다.

## 사용 시점

- clip-path 기반 도형 변환 애니메이션 구현 시
- 마우스 커서를 추적하는 조명/반응 효과 구현 시
- 시네마틱(영화적) 분위기의 UI 배경 구현 시

## 핵심 기법 요약

| 효과 | 핵심 기술 | 용도 |
|------|----------|------|
| Animated Button | `clip-path` + `@keyframes` + CSS 변수 도형 | 버튼, CTA, 장식 요소 |
| Cinema-like BG | SVG `feSpecularLighting` + `feTurbulence` + Lerp 커서 추적 | 히어로 섹션, 랜딩 배경 |

## 공통 규칙

- CSS 변수(`:root`)로 도형 좌표를 정의하고 `clip-path`에서 참조
- 커서 추적은 `requestAnimationFrame` + Lerp(선형 보간)으로 부드럽게 처리
- SVG 필터 요소는 DOM에 존재하되 시각적으로 숨긴다
- `prefers-reduced-motion` 미디어 쿼리로 접근성 대응 필수

## 상세 참조

- **도형 변환 버튼**: [animated-button.md](animated-button.md) — clip-path 도형 정의, shapeshift keyframes, 그라디언트 이동
- **시네마틱 배경**: [cinema-like.md](cinema-like.md) — SVG 조명 필터, 노이즈 시드 애니메이션, 커서 추적 컴포넌트
