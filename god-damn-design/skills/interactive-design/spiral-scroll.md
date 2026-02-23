# Spiral Scrollytelling

`sibling-index()` + `sibling-count()` + Scroll-Driven Animation을 결합한
나선형 스크롤 연출 패턴이다. JavaScript 없이 순수 CSS로 구현한다.

> **브라우저 지원**: Chrome 117+, Edge 117+ (Firefox 미지원, 2025 기준)
> Scroll-Driven Animation: Chrome 115+

---

## 핵심 함수

### sibling-index() / sibling-count()

형제 요소 내에서 자신의 순서와 전체 개수를 CSS 값으로 반환한다.

```css
.item {
  /* sibling-index(): 0부터 시작하는 자신의 인덱스 */
  /* sibling-count(): 형제 요소 전체 개수 */

  opacity: calc(1 - (1 / sibling-count() * sibling-index()));
  /*         첫 번째 = 1  →  마지막 = 0 으로 점진적 감소 */
}
```

### 기본 공식 패턴

```css
/* 초기값에서 인덱스에 따라 선형으로 감소 */
속성값 = 초기값 - ((감소량 / sibling-count()) * sibling-index())

/* 예: 반지름 10vh → 3vh 로 줄어드는 경우 */
--radius: calc(10vh - (7vh / sibling-count() * sibling-index()));
```

---

## 1. 나선형 텍스트 레이아웃

### 원리

각 문자에 두 CSS 변수를 부여한다.

- `--rotation` : 몇 바퀴를 돌며 배치할지 (3바퀴 = 360° × 3)
- `--radius` : 중심에 가까울수록 작아지는 반지름

```css
.char {
  /* 3바퀴 회전하며 균등 배치 */
  --rotation: calc((360deg * 3 / sibling-count()) * sibling-index());

  /* 바깥(10vh) → 안(3vh)으로 좁아지는 반지름 */
  --radius: calc(10vh - (7vh / sibling-count() * sibling-index()));

  position: absolute;
  top: 50%;
  left: 50%;
  transform:
    rotate(var(--rotation))
    translateY(calc(-2.9 * var(--radius)))
    scale(calc(0.4 - (0.25 / sibling-count()) * sibling-index()));
}
```

### HTML 구조

텍스트를 문자 단위 span으로 분리한다.

```html
<div class="vortex">
  <span class="char">H</span>
  <span class="char">e</span>
  <span class="char">l</span>
  <span class="char">l</span>
  <span class="char">o</span>
  <!-- ... -->
</div>
```

JS로 자동 분리하는 경우 (GSAP SplitText 없이):

```javascript
function splitToChars(el) {
  const text = el.textContent;
  el.innerHTML = [...text]
    .map(ch => `<span class="char">${ch === ' ' ? '&nbsp;' : ch}</span>`)
    .join('');
}

splitToChars(document.querySelector('.vortex'));
```

---

## 2. Scroll-Driven Animation 연결

`animation-timeline: scroll()`을 사용하면 스크롤 위치가
애니메이션 진행도를 직접 제어한다. JavaScript 이벤트 리스너 불필요.

### 전체 뷰텍스 회전 (부모)

```css
@keyframes vortex {
  from {
    transform: scale(1) rotate(0deg);
    opacity: 1;
  }
  to {
    transform: scale(0.1) rotate(-720deg);
    opacity: 0;
  }
}

.vortex {
  position: fixed;
  left: 50%;
  top: 0;
  height: 100vh;

  animation-name: vortex;
  animation-duration: 1s;          /* scroll-driven에서 duration은 무시됨 */
  animation-fill-mode: forwards;
  animation-timing-function: linear;
  animation-timeline: scroll();    /* 문서 스크롤에 연결 */
}
```

### 문자별 페이드인 스태거 (자식)

각 문자가 스크롤 진행도에 따라 순서대로 등장한다.

```css
@keyframes fade-in {
  from { opacity: 0; filter: blur(8px); }
  to   { opacity: 1; filter: blur(0);   }
}

.char {
  /* 앞서 정의한 나선 transform 유지 */
  --rotation: calc((360deg * 3 / sibling-count()) * sibling-index());
  --radius:   calc(10vh - (7vh / sibling-count() * sibling-index()));

  position: absolute;
  top: 50%;
  left: 50%;
  transform:
    rotate(var(--rotation))
    translateY(calc(-2.9 * var(--radius)))
    scale(calc(0.4 - (0.25 / sibling-count()) * sibling-index()));

  animation-name: fade-in;
  animation-fill-mode: forwards;
  animation-timing-function: ease-out;
  animation-timeline: scroll();

  /* 인덱스에 따라 페이드인 시작 시점을 분산 */
  animation-range-start: calc(90% / sibling-count() * sibling-index());
  animation-range-end:   calc(90% / sibling-count() * (sibling-index() + 3));
}
```

### 스크롤 컨테이너 세팅

```css
/* 스크롤 공간 확보 */
body {
  height: 400vh;   /* 스크롤 길이 = 연출 시간 */
}

.vortex {
  position: fixed;  /* 화면에 고정, 스크롤로 애니메이션만 제어 */
}
```

---

## 3. sibling-index() 응용 패턴

나선 외에도 인덱스 기반 점진 스타일링에 폭넓게 쓸 수 있다.

### 점진 색상 (hue 분산)

```css
.item {
  background: oklch(
    65%
    0.2
    calc(360deg / sibling-count() * sibling-index())
  );
}
```

### 점진 지연 (stagger delay)

```css
.item {
  animation-delay: calc(0.1s * sibling-index());
}
```

### 점진 불투명도

```css
.item {
  opacity: calc(1 - 0.8 / sibling-count() * sibling-index());
}
```

### 점진 크기

```css
.item {
  scale: calc(1 - 0.5 / sibling-count() * sibling-index());
}
```

### 원형 배치 (cos/sin 연계)

```css
/* css-trig 스킬의 원형 레이아웃을 sibling-index()로 대체 가능 */
.item {
  --angle: calc(360deg / sibling-count() * sibling-index());
  --r: 10rem;
  transform:
    translateX(calc(cos(var(--angle)) * var(--r)))
    translateY(calc(sin(var(--angle)) * var(--r)));
}
```

---

## 4. React 컴포넌트

```tsx
interface SpiralTextProps {
  text: string;
  turns?: number;       // 나선 바퀴 수 (기본 3)
  radiusStart?: string; // 바깥 반지름 (기본 '10vh')
  radiusEnd?: string;   // 안쪽 반지름 (기본 '3vh')
  className?: string;
}

const SpiralText: React.FC<SpiralTextProps> = ({
  text,
  turns = 3,
  radiusStart = '10vh',
  radiusEnd = '3vh',
  className,
}) => {
  const chars = [...text];

  return (
    <>
      <style>{`
        @keyframes vortex {
          from { transform: translate(-50%, -50%) scale(1) rotate(0deg); opacity: 1; }
          to   { transform: translate(-50%, -50%) scale(0.1) rotate(-720deg); opacity: 0; }
        }
        @keyframes char-fade-in {
          from { opacity: 0; filter: blur(8px); }
          to   { opacity: 1; filter: blur(0); }
        }
      `}</style>

      <div
        className={className}
        style={{
          position: 'fixed',
          left: '50%',
          top: '50%',
          animationName: 'vortex',
          animationDuration: '1s',
          animationFillMode: 'forwards',
          animationTimingFunction: 'linear',
          animationTimeline: 'scroll()',
        }}
      >
        {chars.map((ch, i) => {
          const total = chars.length;
          const angleDeg = (360 * turns / total) * i;
          const angleRad = (angleDeg * Math.PI) / 180;

          // radiusStart ~ radiusEnd 사이를 선형 보간
          // CSS 변수로 완전히 처리하기 어려운 경우 JS 계산
          const rangeStart = `${(90 / total) * i}%`;
          const rangeEnd   = `${(90 / total) * (i + 3)}%`;

          return (
            <span
              key={i}
              style={{
                position: 'absolute',
                top: '50%',
                left: '50%',
                // sibling-index()를 지원하는 환경에서는 CSS 변수로 대체 가능
                transform: `rotate(${angleDeg}deg) translateY(calc(-2.9 * (${radiusStart} - ((${radiusStart} - ${radiusEnd}) / ${total} * ${i})))) scale(${0.4 - (0.25 / total) * i})`,
                animationName: 'char-fade-in',
                animationFillMode: 'forwards',
                animationTimingFunction: 'ease-out',
                animationTimeline: 'scroll()',
                animationRangeStart: rangeStart,
                animationRangeEnd: rangeEnd,
              } as React.CSSProperties}
            >
              {ch === ' ' ? '\u00A0' : ch}
            </span>
          );
        })}
      </div>
    </>
  );
};
```

---

## 5. 성능 특성

| 항목 | JavaScript 방식 | CSS 방식 |
|------|----------------|---------|
| 메인 스레드 | 스크롤 이벤트마다 실행 | 관여 없음 |
| GPU 가속 | 수동 will-change 설정 | 자동 처리 |
| 수백 개 요소 | 성능 저하 | 매끄럽게 처리 |
| 코드량 | 많음 | 적음 |

---

## 주의사항

- `sibling-index()` / `sibling-count()` — Firefox 미지원 (2025 기준)
- `animation-timeline: scroll()` — Firefox 110+ 지원, Safari 미지원
- 모바일에서 `vh` 기반 반지름이 작아져 문자가 겹칠 수 있음 → `svh` 또는 고정값 병행 권장
- `animation-range-start/end` 퍼센트는 스크롤 컨테이너 전체 높이 기준
