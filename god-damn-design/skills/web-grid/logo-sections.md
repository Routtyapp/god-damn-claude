# Logo Section Grid Patterns

고객사 로고, 파트너, 수상 내역 등을 나열하는 섹션의 레이아웃 패턴.
신뢰 구축(social proof)이 주목적이며 히어로 섹션 직후에 자주 배치된다.

---

## Pattern 01 — Infinite Marquee (무한 스크롤 로고 띠)

### 레이아웃 구조

```
┌──────────────────────────────────────────── 100vw ────┐
│  [로고A] [로고B] [로고C] [로고D] [로고A] [로고B] ...  │  ← 끊임없이 좌로 이동
└───────────────────────────────────────────────────────┘
   ↑ overflow: hidden 으로 바깥은 잘림
```

### 구조 원리

콘텐츠를 **정확히 2회 반복**한 뒤 `translateX(0) → translateX(-50%)`로 이동.
끝에 닿는 순간 처음 위치와 동일해져 끊김 없이 루프된다.

```
[콘텐츠 ×1][콘텐츠 ×1]  ← 트랙 전체 너비
      -50% 이동 = 콘텐츠 1벌 너비만큼 이동 = 원점으로 돌아옴
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| 컨테이너 폭 | `width: 100vw` + `max-width: 100%` | 뷰포트 전체 폭 |
| 섹션 높이 | `height: 200px` | 콘텐츠 크기에 따라 조정 |
| 트랙 위치 | `position: absolute` + `white-space: nowrap` | 줄바꿈 방지 |
| 애니메이션 | `translateX(0) → translateX(-50%)` | 콘텐츠 1벌 너비 = 50% |
| 속도 | `32s linear infinite` | 느릴수록 우아함, 빠를수록 에너지 |
| 성능 힌트 | `will-change: transform` | GPU 레이어 분리 |

### 핵심 결정

- **`position: absolute` 트랙** — 컨테이너 흐름에서 벗어나 독립적으로 이동
- **`overflow-x: hidden`** — 화면 밖으로 나간 콘텐츠를 잘라 무한 루프처럼 보이게 함
- **`white-space: nowrap`** — 줄바꿈 없이 한 줄로 유지해야 -50% 계산이 성립
- **`linear` 타이밍** — ease 계열 사용 시 속도가 불균일해 어색함. 반드시 linear

### 코드 골격

```html
<div class="marquee">
  <div class="track">
    <div class="content">
      로고A &nbsp; 로고B &nbsp; 로고C &nbsp;
      로고A &nbsp; 로고B &nbsp; 로고C &nbsp;  <!-- 동일 내용 2회 -->
    </div>
  </div>
</div>
```

```css
.marquee {
  position: relative;
  width: 100vw;
  max-width: 100%;
  height: 200px;        /* 로고 크기에 맞게 조정 */
  overflow-x: hidden;
}

.track {
  position: absolute;
  white-space: nowrap;
  will-change: transform;
  animation: marquee 32s linear infinite;
}

@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}

/* 접근성 */
@media (prefers-reduced-motion: reduce) {
  .track { animation: none; }
}
```

### 변형

| 변형 | 방법 |
|------|------|
| 역방향 | `animation-direction: reverse` |
| 호버 시 정지 | `.track:hover { animation-play-state: paused; }` |
| 두 줄 엇갈림 | 같은 마키 2개, 하나는 reverse — 리듬감 생성 |
| 속도 조절 | 로고 수 많으면 느리게(40s+), 적으면 빠르게(15s~) |

### 사용 맥락

- 고객사 로고 10개 이상 (정적 그리드로 나열하기엔 너무 많을 때)
- "우리 서비스를 쓰는 브랜드들" 신뢰 증거 섹션
- 큰 타이포 슬로건 반복으로 브랜드 에너지 전달 시
- 히어로 하단, 또는 섹션 전환부의 리듬 요소로

---
