---
name: pattern-design
description: CSS background-image와 SVG data-uri를 활용한 반복 패턴 배경 구현 가이드. 격자(Grid) 패턴에 원·다이아몬드·별 등 임의 SVG 도형을 renderPattern 콜백으로 자유롭게 지정할 수 있다. backgroundImage + backgroundSize 기반 타일링 패턴이 필요할 때 이 스킬을 참조한다.
---

# Pattern Design

CSS `background-image`와 인라인 SVG를 결합한 반복 패턴 배경 스킬이다.

## 사용 시점

- 격자 + 도형 조합의 반복 배경 구현 시
- 브랜드 아이덴티티용 커스텀 패턴 배경 구현 시
- SVG data-uri 기반 타일링 패턴이 필요할 때

## 핵심 원리

1. **SVG data-uri**: 인라인 SVG를 `url("data:image/svg+xml,...")` 형태로 `background-image`에 삽입
2. **격자선**: `linear-gradient`로 수평/수직선을 생성
3. **타일링**: `background-size`로 반복 단위 설정, `background-repeat: repeat`

## 조절 속성

| 속성 | 역할 | 효과 |
|------|------|------|
| `gridSize` | 격자 한 칸의 정방형 크기 | 클수록 듬성듬성 |
| `lineWidth` | 격자선 두께 | 1이면 섬세, 5+이면 팝아트 |
| `patternSize` | 문양 크기 | 격자 대비 도형 비율 조절 |
| `patternColor` | 문양 색상 | `ctx.color`로 렌더 함수에 전달 |
| `renderPattern` | SVG 요소 렌더 함수 | 내장 프리셋 또는 완전 커스텀 도형 |

## 상세 참조

- **격자 패턴**: [grid.md](grid.md) — `renderPattern` 콜백, 내장 프리셋(plus·squircle·circle·diamond·cross·triangle), 커스텀 SVG 예시
- **Blob 패턴**: [blob.md](blob.md) — CSS border-radius blob, SVG path blob, morph 애니메이션
