---
name: 3d-transform
description: CSS 3D 변환 효과 구현 가이드. 카드 뒤집기(card flip), 마우스 추적 tilt, translateZ 기반 깊이감 레이어링 구현 시 참조한다. perspective, preserve-3d, backface-visibility 핵심 속성 포함.
---

# 3D Transform

CSS `transform-style: preserve-3d`와 `perspective`를 활용한 입체 UI 효과.

## 사용 시점

- 카드 앞/뒤 정보를 flip으로 전환할 때
- 마우스 위치에 따라 카드/패널이 기울어지는 효과가 필요할 때
- 레이어 깊이감(z-depth)으로 계층 구조를 시각적으로 표현할 때

## 핵심 3개 속성

```css
perspective: 800px;             /* 부모에 설정 — 소실점 거리 */
transform-style: preserve-3d;  /* 자식 3D 공간 유지 */
backface-visibility: hidden;    /* 뒷면 숨기기 */
```

## perspective 값 가이드

| 값 | 효과 |
|----|------|
| 400px | 강한 원근감, 극적 |
| 800px | 자연스러운 기울기 |
| 1200px | 미묘하고 부드러운 효과 |

## 상세 참조

- **카드 플립**: [card-flip.md](card-flip.md) — rotateY 180° 양면 카드
- **Tilt 효과**: [tilt.md](tilt.md) — mousemove 기반 rotateX/Y + spotlight
- **깊이 레이어**: [depth-stack.md](depth-stack.md) — translateZ 다층 구성
