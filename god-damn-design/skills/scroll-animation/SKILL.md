---
name: scroll-animation
description: 스크롤 연동 애니메이션 가이드. IntersectionObserver 기반 reveal, parallax, CSS scroll-driven animation(View Timeline API) 구현 시 참조한다. 성능 최적화(transform/opacity only, will-change, RAF), prefers-reduced-motion 대응 포함.
---

# Scroll Animation

스크롤 위치와 연동된 요소 등장·이동·변형 애니메이션 패턴이다.

## 사용 시점

- 섹션 진입 시 요소를 자연스럽게 등장시킬 때
- 스크롤에 따라 배경/레이어를 다른 속도로 이동(parallax)할 때
- JS 없이 CSS만으로 스크롤 진행도에 연동된 애니메이션이 필요할 때

## 방법 선택 기준

| 상황 | 권장 방법 |
|------|----------|
| 뷰포트 진입 시 1회 실행 | IntersectionObserver |
| 스크롤 진행도에 비례한 연속 변화 | CSS `animation-timeline: scroll()` |
| 요소가 뷰포트 안에서 이동하는 구간 | CSS `animation-timeline: view()` |
| 배경 레이어 패럴랙스 | scroll event + RAF + Lerp |

## 핵심 성능 규칙

```
transform, opacity 만 사용 — top/left/width/height 금지
will-change: transform 은 애니메이션 직전에만, 평상시 제거
scroll event → RAF로 throttle
```

## 상세 참조

- **Reveal**: [reveal.md](reveal.md) — IntersectionObserver fade/slide/stagger
- **Parallax**: [parallax.md](parallax.md) — 배경 레이어 속도 분리
- **Scroll-driven**: [scroll-driven.md](scroll-driven.md) — CSS View Timeline API
