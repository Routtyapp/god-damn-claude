---
name: components-design
description: 고급 UI 컴포넌트 패턴 구현 가이드. Masonry 레이아웃, Infinite Marquee 등 표준 CSS/라이브러리만으로 구현하기 어려운 복잡한 UI 패턴을 다룬다. 고객사 로고 나열, 불규칙 갤러리, 끊김 없는 텍스트/로고 스크롤 등 다채로운 프론트엔드 컴포넌트가 필요할 때 사용한다.
---

# Components Design

표준 Flexbox/Grid나 기본 컴포넌트로 구현하기 어려운 고급 UI 패턴 스킬이다.

## 사용 시점

- Pinterest 스타일 불규칙 높이 갤러리(Masonry) 구현 시
- 고객사 로고, 슬로건 등을 끊김 없이 무한 반복 스크롤(Marquee)할 때
- 중요 아이템을 여러 트랙에 걸쳐(span) 배치할 때
- 소스 순서와 시각적 배치 순서를 분리해야 할 때

## 핵심 패턴 요약

| 패턴 | 핵심 기술 | 용도 |
|------|----------|------|
| Masonry (열 기반) | `display: masonry` + `grid-template-columns` | 이미지 갤러리, 피드 |
| Masonry (행 기반) | `display: masonry` + `masonry-direction: row` | 수평 스크롤 카드 |
| 트랙 걸치기 | `grid-column: span N` | 강조 아이템, 풀블리드 배너 |
| 배치 공차 | `item-tolerance` | 소스 순서 우선 배치 |
| Infinite Marquee | `translateX(-50%)` 루프 + CSS 변수 속도 | 로고 스트립, 슬로건 띠 |

## 브라우저 지원 주의

`display: masonry`는 **실험적 스펙**이다.
- Chrome 126+ (플래그 활성화 필요)
- 프로덕션에서는 JS 폴백(`masonry-layout` 라이브러리 또는 multi-column) 병행 고려

## 상세 참조

- **Masonry 레이아웃**: [masonry.md](masonry.md) — 기본 컨테이너, 기본 트랙 크기, 트랙 걸치기, item-tolerance 배치 미세조정
- **Infinite Marquee**: [marquee.md](marquee.md) — 구조 원리, React 컴포넌트, 방향/속도/일시정지, 접근성 처리
