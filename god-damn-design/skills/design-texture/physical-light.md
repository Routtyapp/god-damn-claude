# Physical Light Simulation

빛이 물체의 좁은 틈(slit)을 통과해 표면에 퍼지는 물리 현상을 순수 CSS로 재현하는 패턴.
JS 없이 `rotateX`, `linear-gradient`, `filter: blur`, `:has()` 만으로 구현한다.

> Inspired by Camden @ https://x.com/Designownow_/status/1921052041520041991

---

## 구조 원칙

```
.card
├── .light-layer          빛 효과 전용 레이어 (pointer-events: none)
│   ├── .slit             빛이 통과하는 슬릿 — rotateX로 3D 각도
│   ├── .lumen            빛이 퍼지는 레이어 (거리별 세기: min / mid / hi)
│   └── .darken           빛으로 생기는 그림자 레이어
└── .content              실제 콘텐츠
```

빛 효과는 항상 콘텐츠와 분리된 전용 레이어에 배치한다.
`.light-layer`는 `pointer-events: none`으로 인터랙션 차단 필수.

---

## 핵심 기법

| 기법 | 역할 |
|------|------|
| `transform-style: preserve-3d` + `rotateX` | 슬릿과 빛 레이어를 3D 공간에 배치 |
| `linear-gradient(#fff0, #fffa)` | 빛의 거리별 감쇠(falloff) 표현 |
| `filter: blur()` | 빛 확산(diffusion) 소프트닝 |
| `opacity` 단계적 조절 | min(약) / mid(중) / hi(강) 빛 세기 분리 |
| `:has(.toggle.active)` | JS 없이 조명 on/off 상태 전체 카드에 전파 |
| `box-shadow` 다층 | 카드 재질의 반사·굴절·깊이 표현 |

---

## 전체 구현

### HTML

```html
<div class="card">
  <div class="light-layer">
    <div class="slit"></div>
    <div class="lumen">
      <div class="min"></div>
      <div class="mid"></div>
      <div class="hi"></div>
    </div>
    <div class="darken">
      <div class="sl"></div>
      <div class="ll"></div>
      <div class="slt"></div>
      <div class="srt"></div>
    </div>
  </div>
  <div class="content">
    <!-- 아이콘 등 콘텐츠 -->
    <div class="bottom">
      <h4>제목</h4>
      <p>설명 텍스트</p>
      <!-- 조명 토글 -->
      <div class="toggle" onclick="this.classList.toggle('active')">
        <div class="handle"></div>
        <span>Activate Lumen</span>
      </div>
    </div>
  </div>
</div>
```

### CSS

```css
/* ── 카드 기본 재질 ── */
.card {
  position: relative;
  width: 18rem;
  height: 24rem;
  border-radius: 1.8rem;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: end;
  color: #fff;

  /* 조명 OFF 상태 — 외곽 광택만 */
  background: radial-gradient(circle at 50% 0%, #3a3a3a 0%, #1a1a1a 64%);
  box-shadow:
    inset 0  1.01rem 0.2rem -1rem #fff0,   /* 상단 내부 반사 (꺼짐) */
    inset 0 -1.01rem 0.2rem -1rem #0000,   /* 하단 내부 그림자 */
    0 0 0 1px #fff3,                        /* 외곽 테두리 광택 */
    0 4px 4px 0 #0004,                      /* 드롭 섀도우 */
    0 0 0 1px #333;                         /* 외곽 테두리 */

  transition: box-shadow 0.4s ease-in-out, translate 0.4s ease-out;
}

/* 카드 모서리 컷 + 보조 광택 테두리 */
.card::before {
  content: "";
  display: block;
  --offset: 1rem;
  --ax: 4rem;    /* 모서리 컷 크기 */
  width: calc(100% + 2 * var(--offset));
  height: calc(100% + 2 * var(--offset));
  position: absolute;
  left: calc(-1 * var(--offset));
  top: calc(-1 * var(--offset));
  box-shadow: inset 0 0 0 0.06rem #fff2;
  border-radius: 2.6rem;
  clip-path: polygon(
    var(--ax) 0, 0 0, 0 var(--ax),
    var(--ax) var(--ax), var(--ax) calc(100% - var(--ax)),
    0 calc(100% - var(--ax)), 0 100%, var(--ax) 100%,
    var(--ax) calc(100% - var(--ax)), calc(100% - var(--ax)) calc(100% - var(--ax)),
    calc(100% - var(--ax)) 100%, 100% 100%, 100% calc(100% - var(--ax)),
    calc(100% - var(--ax)) calc(100% - var(--ax)), calc(100% - var(--ax)) var(--ax),
    100% var(--ax), 100% 0, calc(100% - var(--ax)) 0,
    calc(100% - var(--ax)) var(--ax), var(--ax) var(--ax)
  );
  transition: all 0.4s ease-in-out;
}

.card:hover { translate: 0 -0.2rem; }
.card:hover::before { --offset: 0.5rem; --ax: 8rem; }

/* ── 조명 레이어 ── */
.light-layer {
  position: absolute;
  inset: 0;
  transform-style: preserve-3d;
  perspective: 400px;
  pointer-events: none;
}

/* 슬릿 — 빛이 통과하는 좁은 틈 */
.slit {
  position: absolute;
  inset: 0;
  margin: auto;
  width: 64%;
  height: 1.2rem;
  background: #121212;       /* OFF 상태: 어두운 틈 */
  box-shadow: 0 0 4px 0 #fff0;
  transform: rotateX(-76deg); /* 3D 각도로 기울여 틈처럼 보임 */
  transition: all 0.4s ease-in-out;
}

/* 빛 확산 레이어 (초기: 숨김) */
.lumen {
  position: absolute;
  inset: 0;
  opacity: 0;
  transition: opacity 0.4s ease-in-out;
}

/* 거리별 빛 세기 — 가까울수록 강하고 좁음 */
.lumen .min {
  width: 70%; height: 3rem;
  background: linear-gradient(#fff0, #fffa);
  position: absolute; inset: 0; bottom: 2.5rem; margin: auto;
  transform: rotateX(-42deg);
  opacity: 0.4;
}
.lumen .mid {
  width: 74%; height: 13rem;
  background: linear-gradient(#fff0, #fffa);
  position: absolute; inset: 0; bottom: 10em; margin: auto;
  transform: rotateX(-42deg);
  filter: blur(1rem);
  opacity: 0.8;
  border-radius: 100% 100% 0 0;
}
.lumen .hi {
  width: 50%; height: 13rem;
  background: linear-gradient(#fff0, #fffa);
  position: absolute; inset: 0; bottom: 12em; margin: auto;
  transform: rotateX(22deg);
  filter: blur(1rem);
  opacity: 0.6;
  border-radius: 100% 100% 0 0;
}

/* 그림자 레이어 — 빛이 만드는 음영 */
.darken {
  position: absolute; inset: 0;
  opacity: 0.5;
  transition: opacity 0.4s ease-in-out;
}
.darken > * { transition: opacity 0.4s ease-in-out; }

/* 중앙 하단 그림자 */
.darken .sl {
  width: 64%; height: 10rem;
  background: linear-gradient(#000, #0000);
  position: absolute; inset: 0; top: 9.6em; margin: auto;
  filter: blur(0.2rem); opacity: 0.1;
  border-radius: 0 0 100% 100%;
  transform: rotateX(-22deg);
}
.darken .ll {
  width: 62%; height: 10rem;
  background: linear-gradient(#000a, #0000);
  position: absolute; inset: 0; top: 11em; margin: auto;
  filter: blur(0.8rem); opacity: 0.4;
  border-radius: 0 0 100% 100%;
  transform: rotateX(22deg);
}

/* 좌우 측면 그림자 (빛 경계선) */
.darken .slt {
  width: 0.5rem; height: 4rem;
  background: linear-gradient(#0005, #0000);
  position: absolute; top: 3.9em; left: 0; right: 11.5rem; margin: auto;
  opacity: 0.6; border-radius: 0 0 100% 100%;
  transform: skewY(42deg);
}
.darken .srt {
  width: 0.5rem; height: 4rem;
  background: linear-gradient(#0005, #0000);
  position: absolute; top: 3.9em; right: 0; left: 11.5rem; margin: auto;
  opacity: 0.6; border-radius: 0 0 100% 100%;
  transform: skewY(-42deg);
}

/* ── 조명 ON 상태 (:has로 JS 없이 전파) ── */
.card:has(.toggle.active) {
  /* 상단 내부 반사 활성화 */
  box-shadow:
    inset 0  1.01rem 0.1rem -1rem #fffa,
    inset 0 -4rem   3rem   -3rem #000a,
    0 -1.02rem 0.2rem -1rem #fffa,
    0  1rem   0.2rem -1rem #000,
    0 0 0 1px #fff2,
    0 4px 4px 0 #0004,
    0 0 0 1px #333;
}

.card:has(.toggle.active) .slit {
  background: #fff;
  box-shadow: 0 0 4px 0 #fff;  /* 슬릿 발광 */
}

.card:has(.toggle.active) .lumen  { opacity: 0.5; }
.card:has(.toggle.active) .darken { opacity: 0.8; }
.card:has(.toggle.active) .darken .sl  { opacity: 0.2; }
.card:has(.toggle.active) .darken .ll  { opacity: 1; }
.card:has(.toggle.active) .darken .slt { opacity: 1; }
.card:has(.toggle.active) .darken .srt { opacity: 1; }

/* 아이콘 조명 반사 */
.card:has(.toggle.active) .icon {
  filter: drop-shadow(0 -1.2rem 2px #0003) brightness(1.64);
}
```

---

## 조절 포인트

| 속성 | 조절 방향 |
|------|----------|
| `.slit` `width` | 넓을수록 빛 폭 넓음 (40~80%) |
| `.slit` `rotateX` | 값이 클수록 더 기울어진 각도 (60~85deg) |
| `.lumen .mid` `width` | 빛 퍼짐 범위 조절 |
| `.lumen` `opacity` | 조명 ON 시 밝기 (0.3~0.8) |
| `.lumen .mid` `filter: blur()` | 빛 부드러움 (0.5~2rem) |
| `perspective` | 낮을수록 원근감 강함 (300~600px) |

---

## 사용 시나리오

| 시나리오 | 적용 방식 |
|----------|----------|
| 프리미엄 제품 카드 | 기본 구조 그대로 |
| 설정 / 활성화 패널 | toggle → 기능 활성 상태 시각화 |
| 히어로 섹션 장식 | slit + lumen만 분리해 배경 요소로 |
| 다크 테마 강조 요소 | 조명 ON 상태를 hover에 연결 |

---

## 주의사항

- `transform-style: preserve-3d` 레이어에 `overflow: hidden` 금지
- `perspective`는 `.light-layer`에만 설정 — 카드 전체에 걸면 레이아웃 왜곡
- `:has()` — Chrome 105+, Safari 15.4+. 미지원 시 JS 클래스 토글로 폴백
- `filter: blur()` 남발 시 GPU 부하 — `.lumen` 레이어 수 3개 이하 유지
- `pointer-events: none` — `.light-layer` 전체 필수 (클릭 이벤트 차단)
