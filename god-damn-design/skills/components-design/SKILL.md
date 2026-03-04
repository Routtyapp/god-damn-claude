---
name: components-design
description: 고급 UI 컴포넌트 패턴 구현 가이드. Masonry 레이아웃 등 CSS 표준 밖의 복잡한 UI 패턴을 구현할 때 참조한다. display:masonry, 트랙 걸치기, item-tolerance 등 최신 CSS 레이아웃 스펙이 필요할 때 이 스킬을 사용한다.
---

# Components Design

표준 Flexbox/Grid로 구현하기 어려운 고급 UI 레이아웃 패턴 구현 스킬이다.

## 사용 시점

- Pinterest 스타일 불규칙 높이 갤러리(Masonry) 구현 시
- 중요 아이템을 여러 트랙에 걸쳐(span) 배치할 때
- 소스 순서와 시각적 배치 순서를 분리해야 할 때

## 핵심 패턴 요약

| 패턴 | 핵심 CSS | 용도 |
|------|----------|------|
| Masonry (열 기반) | `display: masonry` + `grid-template-columns` | 이미지 갤러리, 피드 |
| Masonry (행 기반) | `display: masonry` + `masonry-direction: row` | 수평 스크롤 카드 |
| 트랙 걸치기 | `grid-column: span N` | 강조 아이템, 풀블리드 배너 |
| 배치 공차 | `item-tolerance` | 소스 순서 우선 배치 |

## 브라우저 지원 주의

`display: masonry`는 **실험적 스펙**이다.
- Chrome 126+ (플래그 활성화 필요)
- 프로덕션에서는 JS 폴백(`masonry-layout` 라이브러리 또는 multi-column) 병행 고려

## 상세 참조

- **Masonry 레이아웃**: [masonry.md](masonry.md) — 기본 컨테이너, 기본 트랙 크기, 트랙 걸치기, item-tolerance 배치 미세조정
