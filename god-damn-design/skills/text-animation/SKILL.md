---
name: text-animation
description: 타이포그래피 애니메이션 구현 가이드. 타이프라이터, 텍스트 scramble, 글자별 reveal, 그라디언트 흐르는 텍스트, 단어 슬라이드 전환 구현 시 참조한다. aria-label로 스크린리더 접근성 보장, prefers-reduced-motion 대응 포함.
---

# Text Animation

텍스트 자체를 인터랙티브하게 만드는 타이포그래피 애니메이션 패턴.

## 사용 시점

- 히어로 섹션에서 타이핑되는 텍스트로 주목도를 높일 때
- 값이 변하는 단어를 회전 전환으로 표현할 때
- 스크롤 등장 시 글자별로 순차 등장하는 효과가 필요할 때
- 텍스트에 그라디언트 흐름 효과로 시각적 생동감을 줄 때
- 스크롤 진입 시 특정 단어/문장을 형광펜으로 칠하듯 강조할 때

## 접근성 원칙

```tsx
// 애니메이션 텍스트는 aria-label로 실제 내용을 스크린리더에 전달
<div aria-label="실제 텍스트 내용">
  {/* 분해된 span들은 aria-hidden */}
  {'글자'.split('').map((c, i) => <span key={i} aria-hidden="true">{c}</span>)}
</div>
```

## 상세 참조

- **타이프라이터**: [typewriter.md](typewriter.md) — 단일/다중 문장 타이핑 + 커서
- **Scramble**: [scramble.md](scramble.md) — 랜덤 문자 해독 효과
- **글자 Reveal**: [char-reveal.md](char-reveal.md) — 캐릭터별 등장, 단어 회전, blur reveal
- **그라디언트 텍스트**: [gradient-text.md](gradient-text.md) — 흐르는 그라디언트, shine 효과
- **하이라이트 Reveal**: [highlight-reveal.md](highlight-reveal.md) — background-size wipe-in, background/half/underline 3가지 스타일
