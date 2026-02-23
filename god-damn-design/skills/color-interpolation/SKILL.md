---
name: color-interpolation
description: CSS 색상 보간(Color Interpolation) 가이드. color-mix(), linear-gradient(), animation에서 색상 공간(oklch, srgb 등)과 hue 보간 방향(shorter, longer, increasing, decreasing)을 지정해 선명하고 아름다운 색상 전환을 구현할 때 사용한다.
---

# CSS Color Interpolation

두 색상 사이의 중간값을 어떤 색상 공간에서, 어떤 경로로 계산할지 제어하는 스킬이다.

## 사용 시점

- 그라디언트 중간색이 탁하거나 회색빛이 돌 때
- `color-mix()`로 색상을 조합할 때
- 애니메이션 색상 전환을 선명하게 만들고 싶을 때
- 무지개 효과 또는 특정 방향의 hue 전환이 필요할 때

## 브라우저 지원

Chrome 111+, Firefox 113+, Safari 16.2+ — 모던 브라우저 전체 지원

## 상세 참조

- [color-interpolation.md](color-interpolation.md) — 색상 공간, hue 메서드, 전체 사용 패턴
