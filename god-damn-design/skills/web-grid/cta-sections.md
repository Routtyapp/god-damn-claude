# CTA Section Grid Patterns

행동 유도(Call-to-Action) 섹션의 레이아웃 패턴.
점수 카드, 가격표, 플랜 비교 등 사용자의 결정을 유도하는 섹션이 이에 해당한다.

---

## Pattern 01 — Score Card CTA (반원 게이지 + 카운트업 + 버튼)

### 레이아웃 구조

```
Desktop (> 860px) — 3컬럼 균등
┌──────────────┬──────────────┬──────────────┐
│  Score Card  │  Score Card  │  Score Card  │
│  (1×1)       │  (1×1)       │  (1×1)       │
└──────────────┴──────────────┴──────────────┘

Mobile (≤ 860px) — 1컬럼, max-width 400px로 카드 폭 제한
┌──────────────────┐
│   Score Card     │
├──────────────────┤
│   Score Card     │
├──────────────────┤
│   Score Card     │
└──────────────────┘
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| 그리드 max-width | `1000px` | |
| 컬럼 | `repeat(3, 1fr)` | 균등 3분할 |
| Gap | `1.5rem` (24px) | |
| 브레이크포인트 | `860px` | 표준 Tailwind bp 아님 — 커스텀 |
| 모바일 max-width | `400px` | 1컬럼 전환 후 카드가 너무 넓어지지 않도록 |
| 카드 padding | `1.75rem` (28px) | |
| 카드 border-radius | `1.25rem` (20px) | |
| 카드 shadow | `0 4px 20px rgba(0,0,0,0.05)` | |

### 카드 내부 구조

```
┌─────────────────────────────┐
│ cardHeader                  │  justify-content: space-between
│   title          [badge]    │
├─────────────────────────────┤
│ chartContainer (180×110px)  │  position: relative
│   SVG 반원 게이지             │
│   scoreData (absolute)      │  bottom: 0, 중앙 정렬
│     숫자 카운트업             │
│     "OUT OF N" 라벨          │
├─────────────────────────────┤
│ description (flex-grow: 1)  │  ← 텍스트가 버튼을 아래로 밀어냄
├─────────────────────────────┤
│ button (w-full)             │  outline 또는 primary
└─────────────────────────────┘
```

**`flex-grow: 1`의 역할:** 카드는 `flex-direction: column`. description에 `flex-grow: 1`을 주면 텍스트가 남은 공간을 채우고 버튼이 항상 카드 하단에 고정된다. 텍스트 길이가 달라도 버튼 높이가 맞춰진다.

### 반원형 SVG 게이지

```
viewBox="0 0 100 65"  ← 반원이 딱 맞게 들어가는 좁은 뷰박스

path: M (50-r) 55 A r r 0 0 1 (50+r) 55
  → radius=46 기준: M 4 55 A 46 46 0 0 1 96 55
  → 좌하단 → 우하단 으로 호를 그리는 반원
```

**strokeDasharray / strokeDashoffset 애니메이션:**

```
circumference = π × radius  (반원 둘레)
= Math.PI × 46 ≈ 144.5

// 초기값 (0%): strokeDashoffset = circumference (선이 전부 숨겨짐)
// 목표값: strokeDashoffset = circumference - (score/maxScore × circumference)
//        = circumference × (1 - score/maxScore)
```

```tsx
const percentage = score / maxScore;
const targetOffset = circumference - (percentage * circumference);
// 100% → offset 0 (선 전부 보임)
// 0%   → offset circumference (선 전부 숨겨짐)
```

### 그라디언트 ID 충돌 방지

같은 페이지에 카드가 여러 개이면 SVG `<defs>` 내 `id`가 겹쳐 다른 카드의 그라디언트를 가져오는 버그 발생.

```tsx
// title을 기반으로 고유 ID 생성
const gradientId = `gradient-${title.replace(/\s+/g, '-').toLowerCase()}`;
// "Credit Score" → "gradient-credit-score"
```

### 카운트업 애니메이션

```tsx
useEffect(() => {
  setStrokeDashoffset(circumference);  // 초기화

  const timer = setTimeout(() => {
    // 게이지 애니메이션
    setStrokeDashoffset(targetOffset);

    // 숫자 카운트업
    let start = 0;
    const stepTime = Math.abs(Math.floor(1500 / score));  // 1.5s 안에 완료
    const counter = setInterval(() => {
      start += 1;
      setCurrentScore(start);
      if (start >= score) clearInterval(counter);
    }, stepTime);
  }, 100);  // 렌더링 완료 후 시작
}, [score]);
```

> `setTimeout(fn, 100)` — 마운트 직후 바로 실행하면 CSS transition이 적용되기 전이라 애니메이션이 안 보임. 100ms 딜레이로 렌더링 완료 보장

### 배지 시스템

```tsx
// 상태에 따라 다른 배지 색상
badgeType: 'moderate' → bg #fff6e5, text #d97706 (Amber — 보통)
badgeType: 'strong'   → bg #e6f6f0, text #059669 (Emerald — 좋음)
badgeType: 'none'     → 배지 없음
```

### 버튼 타입

```tsx
buttonType: 'outline'  → 흰 배경 + border #e5e7eb + 텍스트 #1f2937
buttonType: 'primary'  → bg #3b82f6 (Blue 500) + 텍스트 white
```

카드 3개 중 1개만 `primary` → 시선을 원하는 CTA로 유도

### 코드 골격

```tsx
<ScoreCard
  title="Credit Score"
  badgeText="Moderate"
  badgeType="moderate"
  score={72}
  maxScore={100}
  colorStart="#f59e0b"
  colorEnd="#ef4444"
  description="Your credit score needs some improvement..."
  buttonText="Improve Score"
  buttonType="outline"
/>
```

### 사용 맥락

- 금융, 건강, 성과 측정 앱 (점수 기반 CTA)
- 3가지 지표를 나란히 비교할 때
- 숫자 + 시각적 게이지로 현황 인식 후 행동 유도
- 각 카드마다 다른 우선순위 버튼(outline vs primary)으로 주목도 조절

---

## Pattern 02 — Testimonial Carousel (인용문 슬라이드, JS 반응형)

### 레이아웃 구조

```
Desktop (≥ 1024px) — 3개 동시 표시
┌──────────────┬──────────────┬──────────────┐
│  인용문 1    │  인용문 2    │  인용문 3    │
│              │              │              │
└──────────────┴──────────────┴──────────────┘
                    ← → •••

Tablet (640–1024px) — 2개 / Mobile (< 640px) — 1개
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| 컨테이너 max-width | `1200px` | overflow: hidden |
| 컨테이너 padding | `2rem 0` | 좌우 padding 없음 — 카드가 끝까지 |
| 카드 padding | `2.5rem` | lg / `2rem` md / `1.5rem` mobile |
| 카드 구분 | `border-left: 1px solid #f0f0f0` | gap 아닌 선으로 구분 |
| 카드 min-height | `280px` | 짧은 인용문도 높이 유지 |
| 슬라이드 전환 | `cubic-bezier(0.25, 1, 0.5, 1)` 0.5s | ease-out 계열 — 자연스러운 감속 |
| 반응형 방식 | **JS resize 이벤트** | CSS media query 아님 |
| 도트 크기 | `4px` 기본 → `active: scale(1.5)` | 물리적 dot 이동 없이 scale만 |

### 슬라이드 이동 공식

```
카드 폭 = 100 / itemsPerView (%)

// itemsPerView = 3이면 카드 하나 = 33.3%
// currentIndex 1 이동 = 33.3% 이동

translateX = -(currentIndex × (100 / itemsPerView))%
```

```tsx
// 카드 폭: 인라인 스타일로 동적 지정 (CSS Module로 처리 불가)
style={{ width: `${100 / itemsPerView}%` }}

// 트랙 이동
style={{ transform: `translateX(-${currentIndex * (100 / itemsPerView)}%)` }}
```

### JS 반응형 처리

CSS media query 대신 JS `resize` 이벤트를 사용하는 이유: 카드 폭과 슬라이드 이동량이 `itemsPerView` 상태값에 의존하기 때문. CSS만으로는 JS state 업데이트 불가.

```tsx
useEffect(() => {
  const handleResize = () => {
    if (window.innerWidth < 640)       setItemsPerView(1);
    else if (window.innerWidth < 1024) setItemsPerView(2);
    else                               setItemsPerView(3);
  };

  handleResize(); // 초기 1회 실행
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

### 첫 번째 카드 border 제거

모든 카드에 `border-left`가 있으면 맨 왼쪽 카드에 불필요한 선이 생김.

```tsx
const isFirstVisible = idx === currentIndex || itemsPerView === 1;
// → currentIndex가 바뀔 때마다 재계산
// → mobile(1개 표시)에서는 항상 border 없음
```

### 도트 개수 계산

```tsx
const maxIndex = Math.max(0, totalItems - itemsPerView);
// 5개 testimonials, 3개 표시 → maxIndex = 2
// 도트 수 = maxIndex + 1 = 3개 (인덱스 0, 1, 2)

const dots = Array.from({ length: maxIndex + 1 }, (_, i) => i);
```

> `handleDotClick`에서 `Math.min(index, maxIndex)` 처리 — itemsPerView가 바뀌면 maxIndex도 바뀌므로, 이전 dot 클릭이 범위 초과하지 않도록 보호

### 카드 디자인 원칙

- **배경 transparent** — 컨테이너 배경을 그대로 사용. 카드 자체에 배경 없음
- **border-left로 구분** — gap 대신 선으로 구분. 시각적으로 더 얇고 신문 컬럼 느낌
- **`flex-shrink: 0`** — flex 컨테이너 안에서 카드가 압축되지 않도록 필수
- **`justify-content: space-between`** — 인용문과 저자 정보가 상하로 분리

### 코드 골격

```tsx
// 슬라이드 트랙
<div
  className={styles.sliderTrack}
  style={{ transform: `translateX(-${currentIndex * (100 / itemsPerView)}%)` }}
>
  {testimonials.map((t, idx) => (
    <div
      key={t.id}
      className={`${styles.testimonialCard} ${idx === currentIndex ? styles.firstVisible : ''}`}
      style={{ width: `${100 / itemsPerView}%` }}
    >
      <p className={styles.quoteText}>{t.quote}</p>
      <p className={styles.authorInfo}>
        <span className={styles.authorName}>{t.author}</span>, {t.handle}
      </p>
    </div>
  ))}
</div>

// 도트 네비게이션
{Array.from({ length: maxIndex + 1 }, (_, i) => (
  <button
    key={i}
    className={`${styles.dot} ${currentIndex === i ? styles.active : ''}`}
    onClick={() => setCurrentIndex(Math.min(i, maxIndex))}
  />
))}
```

### Pattern 01 vs Pattern 02

| 항목 | P01 Score Card | P02 Testimonial Carousel |
|------|---------------|--------------------------|
| 레이아웃 | 정적 3열 그리드 | 슬라이딩 캐러셀 |
| 카드 수 | 3개 고정 | N개 (5개+) |
| 반응형 | CSS media query | JS resize 이벤트 |
| 카드 구분 | gap 공백 | border-left 선 |
| 카드 배경 | 흰색 + shadow | transparent |
| 인터랙션 | 없음 (정적) | 슬라이드 + 도트 |
| 브레이크포인트 | 860px (커스텀) | 640/1024px |

### 사용 맥락

- 소셜 프루프(social proof) 섹션 — 고객/사용자 후기
- 인용문이 5개 이상이어서 한 번에 다 보여주기 어려울 때
- 텍스트 중심, 시각적으로 가볍게 유지하고 싶을 때
- 개발자 툴, B2B SaaS — 실제 사용자 목소리로 신뢰 구축

---

## Pattern 03 — Accordion Gallery CTA (Flex 확장 + Glassmorphism 오버레이)

### 레이아웃 구조

```
Desktop — 가로 아코디언 (N개 패널, hover 시 확장)
┌──────┬──────┬──────────────────────────┐
│ P1   │ P2   │ P3 (hover — flex: 5)     │
│      │      │  ┌────────────────────┐  │
│      │      │  │ glassmorphism box  │  │
│      │      │  │ title + desc       │  │
│      │      │  └────────────────────┘  │
└──────┴──────┴──────────────────────────┘

Mobile (≤ 768px) — 세로 스택
┌────────────────────┐
│ Panel 1 (min 80px) │
├────────────────────┤
│ Panel 2            │
├────────────────────┤
│ Panel 3 (active)   │
└────────────────────┘
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| 컨테이너 max-width | `1400px` | 넓은 갤러리에 적합 |
| 갤러리 height | `500px` | tablet: `400px` |
| 패널 기본 flex | `1` | 균등 분배 |
| 패널 확장 flex | `5` | hover/active 시 |
| 패널 min-width | `80px` | 축소 시 너무 좁아지지 않도록 |
| 패널 gap | `1rem` | |
| 패널 border-radius | `1.5rem` | |
| flex 전환 easing | `cubic-bezier(0.25, 1, 0.5, 1) 0.6s` | 자연스러운 스프링감 |
| 이미지 scale (hover) | `1.05` | transition 0.8s ease |
| 오버레이 padding | `2rem` | mobile: `1.5rem` |
| 오버레이 등장 딜레이 | `0.2s` | 패널 확장 후 오버레이 |

### Flex 아코디언 핵심 원리

```css
.galleryWrapper {
  display: flex;
  gap: 1rem;
  height: 500px;
}

.galleryItem {
  flex: 1;
  min-width: 80px;
  transition: flex 0.6s cubic-bezier(0.25, 1, 0.5, 1);
}

.galleryItem:hover {
  flex: 5;
}
```

CSS hover만으로 기본 동작 가능하나, React state(`activeIndex`)와 인라인 스타일을 병행하면 키보드/터치 이벤트 대응이 쉬워진다.

```tsx
// CSS :hover와 React state 병행
style={activeIndex === index ? { flex: 5 } : { flex: 1 }}
onMouseEnter={() => setActiveIndex(index)}
```

> **주의:** CSS `flex` 트랜지션은 `flex-grow`에만 동작한다. `flex: 1`과 `flex: 5`는 실질적으로 `flex-grow: 1/5`로 해석된다.

### Glassmorphism 오버레이 구조

```css
.overlayContent {
  position: absolute;
  bottom: 2rem;
  left: 2rem;
  width: calc(100% - 4rem);
  max-width: 400px;

  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 1rem;

  /* 초기: 숨김 */
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.4s ease 0.2s, transform 0.4s ease 0.2s;
  pointer-events: none;
}

.galleryItem:hover .overlayContent {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}
```

딜레이(`0.2s`)를 주는 이유: 패널이 어느 정도 확장된 후 오버레이가 나타나야 자연스럽다.

→ 자세한 Glassmorphism 구현은 [`design-texture/Glassmorphism.md`](../design-texture/Glassmorphism.md) 참조.

### 헤더 영역 — 타이틀 + 파트너 로고

```
┌─────────────────────────────────────────────────┐
│ h2 (max-width 500px)        [logo][logo][logo]  │
│ 4rem / font-weight 500      flex-wrap, gap 1.5  │
│ letter-spacing -0.04em                          │
└─────────────────────────────────────────────────┘
```

```css
.headerArea {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 3rem;
  flex-wrap: wrap;
  gap: 2rem;
}

.logoBox {
  filter: grayscale(100%);
  opacity: 0.8;
  transition: opacity 0.3s, filter 0.3s;
}
.logoBox:hover { filter: grayscale(0%); opacity: 1; }
```

로고는 grayscale 기본 → hover 시 컬러. 파트너 인지도를 과하지 않게 표시.

### 코드 골격

```tsx
const [activeIndex, setActiveIndex] = useState<number>(0);

<div className={styles.galleryWrapper}>
  {galleryData.map((item, index) => (
    <div
      key={item.id}
      className={styles.galleryItem}
      onMouseEnter={() => setActiveIndex(index)}
      style={activeIndex === index ? { flex: 5 } : { flex: 1 }}
    >
      <img src={item.imageUrl} alt={item.title} className={styles.imageLayer} />
      <div className={styles.overlayContent}>
        <h3 className={styles.overlayTitle}>{item.title}</h3>
        <p className={styles.overlayDesc}>{item.description}</p>
      </div>
    </div>
  ))}
</div>
```

### Pattern 01 / 02 / 03 비교

| 항목 | P01 Score Card | P02 Testimonial Carousel | P03 Accordion Gallery |
|------|---------------|--------------------------|----------------------|
| 레이아웃 | 정적 3열 그리드 | 슬라이딩 캐러셀 | Flex 아코디언 |
| 콘텐츠 | 숫자/게이지 | 인용문 | 이미지 + 텍스트 오버레이 |
| 인터랙션 | 없음 | 슬라이드 | hover expand |
| 반응형 | CSS media query | JS resize | CSS flex-direction 전환 |
| 브레이크포인트 | 860px | 640/1024px | 768/1024px |
| 분위기 | 데이터 중심 | 신뢰/증언 | 비주얼 임팩트 |

### 사용 맥락

- 이미지 중심 섹션 (제조업, 건축, 포트폴리오, 여행)
- 여러 서비스/공간/제품을 동등하게 소개하되, 탐색 행동을 유도할 때
- 파트너사/인증 로고를 함께 노출해 신뢰를 강화할 때
- 3–5개 패널이 최적 (2개는 아코디언 의미 약함, 6개 이상은 min-width가 너무 좁아짐)

---
