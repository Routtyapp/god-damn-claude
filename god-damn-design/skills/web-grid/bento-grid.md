# Bento Grid Patterns

기능·특징을 격자형 카드로 배치하는 제품 쇼케이스 레이아웃.
불균등한 셀 크기로 시각적 위계를 만들고, 각 셀이 독립적인 메시지를 전달한다.

---

## Pattern 01 — Line Bento Grid (1px 라인 구분, 4컬럼)

### 레이아웃 구조

```
Desktop (md: ≥768px) — 4컬럼
┌──────┬──────┬─────────────┐
│  1×1 │  1×1 │    2×1      │
├──────┼──────┴──────┬──────┤
│  1×1 │    2×1      │  1×1 │
├─────────────┬──────┼──────┤
│    2×1      │  1×1 │  1×1 │
└─────────────┴──────┴──────┘

Mobile (< md) — 1컬럼 스택
┌──────────────────┐
│  모든 셀 풀폭     │
│  (aspect 유지)   │
└──────────────────┘
```

### "라인" 효과의 원리

```
gap-[1px] bg-white/10 → 셀 사이 1px 공간이 배경색으로 보임

┌───────┐ ← white/10 배경
│  셀   │← 1px gap
└───────┘
```

`border`나 `divide` 없이, **grid 배경색 + 1px gap**만으로 선을 만든다.

### 셀 크기 조합

| 셀 | 클래스 | 비율 | 비고 |
|----|--------|------|------|
| 1×1 | `col-span-1 aspect-square` | 정방형 | |
| 2×1 | `col-span-1 md:col-span-2 aspect-[2/1]` | 가로 2배 | 모바일은 1컬럼 |

행당 4컬럼을 채우는 조합 패턴:
- `1 + 1 + 2`
- `1 + 2 + 1`
- `2 + 1 + 1`

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| 외부 컨테이너 | `max-w-5xl w-full` | `container` 클래스 아님 — 직접 지정 |
| 외부 패딩 | `p-4 md:p-6` | 16px → 24px |
| 외부 배경 | `bg-[#141414] rounded-3xl` | 카드 래퍼 |
| 그리드 gap | `gap-[1px]` | 라인 효과의 핵심 |
| 그리드 배경 | `bg-white/10` | 1px 공간에 비쳐 선처럼 보임 |
| 그리드 border | `border border-white/10 rounded-xl overflow-hidden` | 외곽선 + 코너 클리핑 |
| 셀 배경 | `bg-[#111]` | 그리드 배경과 대비 |
| 모바일 컬럼 | `grid-cols-1` | 전체 스택 |
| 데스크톱 컬럼 | `md:grid-cols-4` | |

### 핵심 결정

- **`gap-[1px]` + 배경색 = 라인 트릭** — `border`/`divide` 없이 그리드 배경이 1px 틈으로 비침. 컬러 변경 시 `bg-white/10` 하나만 바꾸면 전체 라인 색상 변경
- **`overflow-hidden` on 그리드** — 셀 코너가 그리드 `rounded-xl` 밖으로 나가지 않도록 클리핑
- **`aspect-[2/1]`로 2×1 셀 비율 고정** — 이미지 없이도 셀 높이 일정하게 유지
- **모바일에서 `col-span-2` 해제** — `md:col-span-2`이므로 모바일은 모두 1컬럼. aspect-square, aspect-[2/1] 유지로 비율은 그대로
- **노이즈 배경** — SVG `feTurbulence`를 `opacity-[0.03]`으로 깔아 미세한 질감 추가. 너무 강하면 역효과

### hover 처리

```tsx
// 셀마다 group 추가, 내부에 오버레이 div
<div className="... group">
  <div className="absolute inset-0 bg-white/0 transition-colors duration-500 group-hover:bg-white/5" />
  {/* 콘텐츠 */}
</div>
```

- `bg-white/0 → bg-white/5` — 거의 안 보이는 미세한 밝아짐
- `duration-500` — 빠른 hover보다 느린 전환이 고급스러움
- 그라디언트 셀은 `opacity-60 → opacity-100`

### 코드 골격

```tsx
<div className="max-w-5xl w-full bg-[#141414] p-4 md:p-6 rounded-3xl border border-white/5">
  <div className="grid grid-cols-1 md:grid-cols-4 gap-[1px] bg-white/10 border border-white/10 rounded-xl overflow-hidden">

    {/* 1×1 셀 */}
    <div className="bg-[#111] col-span-1 aspect-square relative overflow-hidden flex items-center justify-center p-6 group">
      <div className="absolute inset-0 bg-white/0 transition-colors duration-500 group-hover:bg-white/5" />
      <span className="relative z-10 text-xs tracking-[0.2em]">LABEL</span>
    </div>

    {/* 2×1 셀 */}
    <div className="bg-[#111] col-span-1 md:col-span-2 aspect-[2/1] relative overflow-hidden flex items-center p-8 group">
      <div className="absolute inset-0 bg-white/0 transition-colors duration-500 group-hover:bg-white/5 z-10" />
      {/* 왼쪽 텍스트 */}
      <span className="relative z-20 text-xs tracking-[0.2em] w-1/2">LABEL</span>
      {/* 오른쪽 비주얼 */}
      <div className="absolute right-0 top-0 bottom-0 w-[60%]">
        {/* SVG 또는 장식 요소 */}
      </div>
    </div>

  </div>
</div>
```

### 셀 내부 비주얼 패턴

| 타입 | 기법 | 용도 |
|------|------|------|
| 텍스트 레이블 | `tracking-[0.2em]` 초광간격 | 기능명, 키워드 |
| 큰 숫자 | `text-8xl font-bold tracking-tighter` | 지표, 배수 |
| SVG 다이어그램 | 인라인 SVG, `opacity-40~90` | 구조 설명 |
| 방사선 패턴 | `Array(36).map` 회전 lines | 장식, 배경 |
| 와이어프레임 | SVG rect + line + circle | 기술적 느낌 |
| 그라디언트 오버레이 | `from-emerald-500/20 via-purple-500/20` | 포인트 셀 |

### 사용 맥락

- 탈중앙화 금융(DeFi), Web3, 크립토 제품
- 다크 테마 기반의 기술 제품 랜딩
- 여러 기능을 동시에 보여줘야 할 때 (6~9개 feature)
- 위계 없이 모든 기능이 동등하게 중요할 때

### 라이트 테마 전환 시

```
bg-[#0a0a0a] → bg-white / bg-gray-50
bg-[#141414] → bg-white / bg-gray-100
bg-[#111]    → bg-white
bg-white/10  → bg-black/10  (라인 색상)
text-white   → text-gray-900
```

---

## Pattern 02 — Colorful Bento Grid (3컬럼, 명시적 배치, 카드별 색상)

### 레이아웃 구조

```
Desktop (> 1024px) — 3컬럼, grid-column/row 명시 배치
┌───────────┬─────────────────────┐
│           │   Scheduling (2×1)  │  row 1
│ Custom    ├──────────┬──────────┤
│ (1×2 tall)│ Wallet   │  Inbox   │  row 2
│           │ (1×1)    │  (1×1)   │
├───────────┴──────────┼──────────┤
│   Send Gifts (2×1)   │Reminders │  row 3
└──────────────────────┴──────────┘

Tablet (≤ 1024px) — 2컬럼, 카드별 개별 override
┌──────────┬──────────┐
│ Custom   │          │
├──────────┤Scheduling│ span 2
│  Wallet  │  Inbox   │
├──────────┴──────────┤
│  Send Gifts (span 2)│
├─────────────────────┤
│  Reminders (span 2) │
└─────────────────────┘

Mobile (≤ 640px) — 1컬럼 전체 스택 (!important)
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| 컨테이너 | `max-w-[1400px] mx-auto` + `py-20 px-4 md:px-8` | |
| 그리드 max-width | `1200px` | 내부 grid만 별도 max-w |
| 컬럼 | `repeat(3, 1fr)` | Pattern 01은 4컬럼 |
| 행 최소 높이 | `grid-auto-rows: minmax(320px, auto)` | 행 높이 하한선 정의 |
| Gap | `1.5rem` (24px) | Pattern 01(1px)과 핵심 차이 — 공백이 구분선 역할 |
| 카드 border-radius | `1.5rem` (24px) | Pattern 01은 셀에 radius 없음 |
| 카드 padding | `2.5rem` (40px) | |
| hover | `translateY(-4px)` + box-shadow | Pattern 01은 opacity 변화 |

### 명시적 배치 공식

`col-span`이 아닌 `grid-column: start / end`, `grid-row: start / end` 직접 지정.

```css
/* 1×2 세로 긴 카드 */
.customizationCard {
  grid-column: 1 / 2;
  grid-row: 1 / 3;      /* 2행 점유 */
}

/* 2×1 가로 긴 카드 */
.schedulingCard {
  grid-column: 2 / 4;   /* col 2~4 점유 */
  grid-row: 1 / 2;
}

/* 1×1 기본 카드 */
.walletCard {
  grid-column: 2 / 3;
  grid-row: 2 / 3;
}
```

> **언제 span 대신 명시 배치를 쓰나:**
> - 세로로 긴 카드(tall card)가 있을 때 — span은 행 점유를 자동으로 못 잡음
> - 카드 순서와 시각 배치를 분리해야 할 때
> - 반응형에서 카드별로 다른 재배치가 필요할 때

### 반응형 전략

```css
/* 태블릿: 2컬럼, 카드별 개별 override */
@media (max-width: 1024px) {
  .bentoContainer { grid-template-columns: repeat(2, 1fr); }
  .customizationCard { grid-column: span 1; grid-row: auto; }  /* tall 해제 */
  .schedulingCard    { grid-column: span 2; }
  .sendGiftsCard     { grid-column: span 2; }
  .remindersCard     { grid-column: span 2; }
}

/* 모바일: 전부 1컬럼, !important로 강제 초기화 */
@media (max-width: 640px) {
  .bentoContainer { grid-template-columns: 1fr; }
  .customizationCard, .schedulingCard, ... {
    grid-column: 1 / -1 !important;  /* 명시 배치 덮어쓰기 */
    grid-row: auto !important;
  }
}
```

> 모바일에서 `!important`가 불가피한 이유: 명시 배치(`grid-column: 1/2`)는 specificity가 높아 단순 `span 1`로 덮어쓰기가 안 됨

### 카드별 색상 시스템

| 카드 | 배경 | 텍스트 | 톤 |
|------|------|--------|----|
| Customization | `#cbb5e8` 라벤더 | `#4a2574` 딥퍼플 | 보라 |
| Scheduling | `#fd95b8` 핑크 | `#a61a5b` 딥핑크 | 핑크 |
| Wallet | `#c6df9b` 연두 | `#274b1e` 딥그린 | 그린 |
| Inbox | `#fce680` 노랑 | `#7a5a08` 딥옐로 | 옐로 |
| Send Gifts | `#ffad7a` 오렌지 | `#66300a` 딥오렌지 | 오렌지 |
| Reminders | `#c8deef` 하늘 | `#1a3a5a` 네이비 | 블루 |

**공식:** 파스텔 배경 + 같은 계열 딥 컬러 텍스트 → 대비 유지 + 부드러운 통일감

### hover 처리

```css
.bentoCard:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 25px -5px rgba(0,0,0,0.1), 0 8px 10px -6px rgba(0,0,0,0.1);
}

/* 카드 내부 UI 목업도 같이 반응 */
.bentoCard:hover .cardMockup         { transform: scale(1.02) translateY(-2px); }
.bentoCard:hover .cardMockupRotate   { transform: scale(1.02) translateY(-2px) rotate(-10deg); }
.bentoCard:hover .cardMockupRotateRight { transform: scale(1.02) translateY(-2px) rotate(8deg); }
```

### Pattern 01 vs Pattern 02

| 항목 | P01 Line Bento | P02 Colorful Bento |
|------|---------------|---------------------|
| 컬럼 수 | 4 | 3 |
| 배치 방법 | `col-span` | `grid-column/row` 명시 |
| 세로 긴 카드 | 불가 | 가능 (`grid-row: 1/3`) |
| Gap | 1px (라인 트릭) | 1.5rem (공백) |
| 카드 배경 | 통일 (`#111`) | 카드별 파스텔 |
| hover | 배경 밝기 변화 | translateY 리프트 |
| 테마 | 다크 | 라이트 |
| 반응형 복잡도 | 낮음 | 높음 (카드별 개별 override) |

### 사용 맥락

- 소비자 앱, 라이프스타일 서비스 (선물, 일정, 지갑 등)
- 라이트 테마, 밝고 따뜻한 브랜드 톤
- 기능 6개 내외, 기능마다 성격이 다를 때
- 세로 긴 카드(tall card)로 콘텐츠 위계가 필요할 때

---

## Pattern 03 — Dark Minimal Bento (텍스트 하단 고정, 비주얼 상단 페이드)

### 레이아웃 구조

```
Desktop (> 1024px) — 3컬럼, 2행, 명시적 배치
┌───────────┬─────────────────────┐
│  Save     │   Search (2×1)      │  row 1
│  (1×1)    │                     │
├───────────┴──────────┬──────────┤
│   Globe (2×1)        │ Calendar │  row 2
└──────────────────────┴──────────┘

Tablet (≤ 1024px) — 2컬럼, 카드별 span 1로 전환
Mobile (≤ 640px)  — 1컬럼 !important 초기화
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| 그리드 max-width | `1100px` | P02(1200px)보다 좁음 |
| 컬럼 | `repeat(3, 1fr)` | |
| 행 최소 높이 | `minmax(380px, auto)` | P02(320px)보다 60px 더 높음 |
| Gap | `1rem` (16px) | P02(1.5rem)보다 작음 |
| 카드 배경 | `#121212` 통일 | P02는 카드별 파스텔 |
| 카드 border | `1px solid rgba(255,255,255,0.05)` | 미세한 외곽선 |
| hover | border 밝아짐 + box-shadow | translateY 없음 |

### 카드 내부 레이어 구조

```
┌─────────────────────────────────┐
│  graphicContainer (absolute)    │  ← z-index: 1, pointer-events: none
│  비주얼(파일카드/검색창/지구/달력)  │
│  ↓ mask-image으로 하단 페이드     │
├─────────────────────────────────┤
│  cardContent (z-index: 10)      │  ← mt-auto → 바닥 고정
│  iconWrapper + title + subtitle │
└─────────────────────────────────┘
```

**핵심 원리:** 카드는 `flex-direction: column`. `cardContent`에 `mt-auto`를 주면 텍스트가 바닥에 붙고, 비주얼은 absolute로 상단을 채운다.

### mask-image 페이드 기법

```css
/* 하단 페이드 — 비주얼 하단이 카드 배경으로 자연스럽게 소멸 */
.fadeBottom {
  mask-image: linear-gradient(to bottom, black 50%, transparent 100%);
}

/* 방사형 페이드 — 지구본처럼 중앙만 선명하게 */
.fadeRadial {
  mask-image: radial-gradient(circle at center, black 40%, transparent 80%);
}
```

비주얼과 텍스트 영역이 자연스럽게 이어지는 이유 — 오버레이나 그라디언트 div 없이 mask 하나로 해결.

### hover 처리

```css
/* 카드 hover: border 밝아짐 + 배경 그림자 */
.bentoCard:hover {
  border-color: rgba(255, 255, 255, 0.15);   /* 0.05 → 0.15 */
  box-shadow: 0 0 40px rgba(0, 0, 0, 0.5);
}

/* 비주얼 그룹 hover: 위로 떠오름 */
.bentoCard:hover .mockupGroup     { transform: translateY(-8px); }
.bentoCard:hover .mockupGroupGlobe { transform: scale(1.05); }
```

P02의 **카드 전체 리프트**와 달리, P03은 **카드는 고정 + 내부 비주얼만 움직임** → 더 미묘하고 세련된 인터랙션

### 코드 골격

```tsx
<div className={styles.bentoCard}>
  {/* 비주얼: absolute 상단 채움 + 하단 페이드 */}
  <div className={`${styles.graphicContainer} ${styles.fadeBottom} flex justify-center pt-8`}>
    <div className={styles.mockupGroup}>
      {/* UI 목업 */}
    </div>
  </div>

  {/* 텍스트: 하단 고정 */}
  <div className={styles.cardContent}>  {/* mt-auto */}
    <div className={styles.iconWrapper}>
      <Icon strokeWidth={1.5} size={28} />
    </div>
    <h2 className={styles.bentoTitle}>Title</h2>
    <p className={styles.bentoSubtitle}>Description</p>
  </div>
</div>
```

### 아이콘 스타일 규칙

- `strokeWidth={1.5}` — 얇은 선 아이콘 (굵으면 무거워 보임)
- `color: #737373` — 텍스트보다 훨씬 낮은 채도, 장식적 역할
- 배경 없음 (`background-color: transparent`) — P02의 컬러 아이콘 배경과 대비

### 3패턴 종합 비교

| 항목 | P01 Line | P02 Colorful | P03 Dark Minimal |
|------|----------|--------------|------------------|
| 컬럼 | 4 | 3 | 3 |
| 행 | 3 | 3 | 2 |
| 카드 수 | 9 | 6 | 4 |
| Gap | 1px | 1.5rem | 1rem |
| 카드 배경 | 통일 다크 | 카드별 파스텔 | 통일 다크 |
| 구분선 | gap 라인 트릭 | 공백 | 카드 border |
| 텍스트 위치 | 중앙/임의 | 상단 | **하단 고정** |
| 비주얼 처리 | 셀 내부 | 인라인 배치 | **absolute + mask** |
| hover | 배경 밝기 | 카드 리프트 | border 글로우 + 내부 이동 |
| 테마 | 다크 | 라이트 | 다크 |

### 사용 맥락

- 개발자 툴, 생산성 앱, 파일 관리 서비스
- 기능이 4개로 적고 각각 시각적 설명이 필요할 때
- 다크 테마 + 미니멀한 분위기
- UI 목업(달력, 검색창, 파일카드)을 데코레이션으로 활용할 때

---
