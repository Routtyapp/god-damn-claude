---
name: micro-interaction
description: 버튼 피드백, 토글, 폼 요소 등 소규모 UI 상태 변화에 대한 인터랙션 가이드. squish press, ripple, spring toggle, floating label, checkbox 애니메이션 구현 시 참조한다. prefers-reduced-motion 대응 및 성능 최적화(transform/opacity only) 포함.
---

# Micro-interaction

사용자 액션에 즉각적이고 자연스러운 시각 피드백을 주는 소규모 애니메이션 패턴이다.

## 사용 시점

- 버튼 클릭/호버에 물리적 피드백이 필요할 때
- 토글/체크박스 등 상태 변환에 생동감을 줄 때
- 폼 필드 포커스·유효성 상태를 시각적으로 표현할 때
- 리다이렉트·탐색 카드에서 "이동하고 싶다"는 여정 욕구를 자극할 때

## 핵심 원칙

| 원칙 | 기준 |
|------|------|
| 즉각성 | 피드백 지연 0ms — 클릭과 동시에 반응 |
| 간결성 | 지속시간 80–400ms, 루프 없음 |
| 물리성 | spring easing `cubic-bezier(0.34, 1.56, 0.64, 1)` |
| 접근성 | `prefers-reduced-motion` 필수 대응 |
| 성능 | `transform`, `opacity` 전용 — layout/paint 유발 금지 |

## 공통 CSS 변수

```css
:root {
  --spring: cubic-bezier(0.34, 1.56, 0.64, 1);
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --duration-fast: 80ms;
  --duration-base: 200ms;
  --duration-spring: 300ms;
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

## CTA / Journey Encouragement (여정 유도 인터랙션)

리다이렉트·탐색 유도 목적의 컴포넌트에서 호버 시 "이동하고 싶다"는 욕구를 강화하는 패턴이다.

### 패턴 1: 방향성 아이콘 이동

아이콘이 호버 시 방향으로 이동하며 다음 여정을 암시한다.

```css
.card-arrow {
  transition: transform var(--duration-base) var(--ease-out);
}
.card:hover .card-arrow {
  transform: translate(3px, -3px); /* 우측 상단 방향 */
}
```

### 패턴 2: 카드 Lift

카드가 위로 살짝 뜨며 클릭 가능성을 암시한다.

```css
.card {
  transition:
    transform var(--duration-base) var(--ease-out),
    box-shadow var(--duration-base) var(--ease-out);
}
.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgb(0 0 0 / 0.12);
}
```

### 패턴 3: 아이콘 Reveal

아이콘이 평소엔 희미하다가 호버 시 선명해지며 의도를 명확히 한다.

```css
.card-icon {
  opacity: 0.4;
  transition:
    opacity var(--duration-fast) var(--ease-out),
    transform var(--duration-base) var(--spring);
}
.card:hover .card-icon {
  opacity: 1;
  transform: scale(1.1);
}
```

### 공통 원칙

- 아이콘 이동 거리: 2–4px (과하지 않게)
- duration: `--duration-base` (200ms) 이하
- `prefers-reduced-motion` 시 opacity 전환만 유지, transform 제거

```css
@media (prefers-reduced-motion: reduce) {
  .card-arrow, .card-icon { transition: opacity var(--duration-fast); }
  .card:hover { transform: none; }
}
```

---

## 상세 참조

- **버튼 피드백**: [button-feedback.md](button-feedback.md) — squish press, ripple, magnetic hover
- **토글 스위치**: [toggle-switch.md](toggle-switch.md) — spring toggle, animated checkbox
- **폼 피드백**: [form-feedback.md](form-feedback.md) — floating label, focus ring, validation state
