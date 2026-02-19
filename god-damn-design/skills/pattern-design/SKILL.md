---
name: pattern-design
description: CSS background-image와 SVG data-uri를 활용한 반복 패턴 배경 구현 가이드. 격자(Grid) 패턴, 도형(십자, 둥근사각형 등) 반복 배경을 React 컴포넌트로 구현할 때 사용한다. backgroundImage + backgroundSize 기반 타일링 패턴이 필요할 때 이 스킬을 참조한다.
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
| `patternType` | 문양 형태 | `plus`, `squircle` 등 |
| `patternSize` | 문양 크기 | 격자 대비 도형 비율 조절 |

## 상세 참조

- **격자 패턴**: [grid.md](grid.md) — 십자/둥근사각형 패턴 React 컴포넌트, 사용 예시
