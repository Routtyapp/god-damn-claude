---
name: css-trig
description: CSS 삼각함수(cos, sin, tan) 활용 가이드. 원형 레이아웃, 파동 배치, 물리 기반 애니메이션, 다각형 섹션 등 수학 기반 UI를 순수 CSS로 구현할 때 사용한다. calc() + CSS 변수와 결합해 마법 숫자 없이 반응형 기하 레이아웃을 설계한다.
---

# CSS Trigonometric Functions

CSS `cos()`, `sin()`, `tan()` 삼각함수로 수학 기반 UI를 구현하는 스킬이다.

## 사용 시점

- 요소를 원형/반원형으로 배치할 때
- 파동(wave) / DNA 나선 레이아웃을 만들 때
- 물리 기반 감쇠(damping) 애니메이션을 만들 때
- 원을 삼각형 조각으로 분할하는 다각형 레이아웃을 만들 때
- 대각선 섹션, 원형 메뉴 등 기하 UI를 구현할 때

## 핵심 원리

```
단위원(radius = 1) 위의 점 좌표
  x = cos(각도)
  y = sin(각도)

반지름 r인 원이면
  x = cos(각도) * r
  y = sin(각도) * r

tan() = sin() / cos()  →  삼각형의 "대변 / 인접변" 비율
```

## 브라우저 지원

Chrome 111+, Firefox 108+, Safari 15.4+ — 현재 모든 모던 브라우저 지원

## 상세 참조

- **cos / sin**: [cos-sin.md](cos-sin.md) — 원형 레이아웃, 파동 배치, 감쇠 애니메이션
- **tan**: [tan.md](tan.md) — 다각형 섹션, 원형 메뉴, 갤러리 슬라이스
