# Hero Section Grid Patterns

실제 코드 기반으로 수집한 히어로 섹션 레이아웃 패턴.
각 패턴은 그리드 구조, 브레이크포인트, 수치를 기록한다.

---

## Pattern 01 — Split Hero (50:50, 텍스트 좌 / 이미지 우)

### 레이아웃 구조

```
Mobile (< 1024px)          Desktop (≥ 1024px / lg)
┌───────────────────┐      ┌──────────────┬──────────────┐
│  텍스트 콘텐츠     │      │  텍스트       │  이미지       │
│  (100%)           │      │  w-1/2       │  w-1/2       │
├───────────────────┤      │  (6 cols)    │  (6 cols)    │
│  이미지 (100%)    │      └──────────────┴──────────────┘
└───────────────────┘
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| Container | Tailwind `container` + `mx-auto` | 브레이크포인트별 자동 max-width |
| 수평 패딩 (Margin) | `px-6` = 24px | 좌우 동일 |
| 히어로 수직 패딩 | `py-16` = 64px | |
| 레이아웃 전환 | `lg` (1024px) | flex 전환 기준 |
| Nav 전환 | `md` (768px) | 햄버거 메뉴 → 수평 메뉴 |
| 텍스트 최대 폭 | `lg:max-w-lg` = 512px | 라인 길이 과도 확장 방지 |
| 컬럼 비율 | 1/2 : 1/2 | 12컬럼 기준 6:6 |

### 핵심 결정

- **모바일에서 이미지가 아래** — 텍스트 → CTA → 이미지 순서로 정보 우선순위 명확
- **이미지에 max-w 없음** — `lg:max-w-3xl`이 있지만 컨테이너 안에서 `w-full`이므로 사실상 컨테이너에 의해 제한
- **nav와 hero의 브레이크포인트 분리** — nav는 md(768px), 레이아웃은 lg(1024px). 태블릿에서 수평 nav이지만 콘텐츠는 여전히 스택

### 코드 골격

```html
<div class="container px-6 py-16 mx-auto">
  <div class="items-center lg:flex">
    <!-- 텍스트: 모바일 full, 데스크톱 50% -->
    <div class="w-full lg:w-1/2">
      <div class="lg:max-w-lg">
        <h1>...</h1>
        <p>...</p>
        <button>CTA</button>
      </div>
    </div>
    <!-- 이미지: 모바일 full + mt-6, 데스크톱 50% -->
    <div class="flex items-center justify-center w-full mt-6 lg:mt-0 lg:w-1/2">
      <img class="w-full h-full lg:max-w-3xl" src="..." />
    </div>
  </div>
</div>
```

### 사용 맥락

- 이커머스, SaaS, 제품 랜딩
- 이미지가 제품/일러스트일 때 (사진보다 SVG/PNG)
- 텍스트 내용이 간결할 때 (H1 + 설명 1줄 + CTA 1개)

---

## Pattern 02 — Centered Hero (텍스트 중앙 + 이미지 하단)

### 레이아웃 구조

```
Mobile & Desktop (레이아웃 전환 없음 — 항상 단일 컬럼)
┌────────────────────────────────────┐
│        텍스트 블록 (max-w-lg)       │  ← mx-auto로 중앙 고정
│    H1 / 설명 / CTA / sub-text      │
├────────────────────────────────────┤
│   이미지  w-full → lg:w-4/5 (80%)  │  ← 데스크톱에서 약간 좁아짐
│   h-96 (384px) fixed, object-cover │
└────────────────────────────────────┘
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| Container | Tailwind `container` + `mx-auto` | |
| 수평 패딩 | `px-6` = 24px | |
| 히어로 수직 패딩 | `py-16` = 64px | |
| 텍스트 블록 최대 폭 | `max-w-lg` = 512px | `mx-auto`로 중앙 정렬 |
| 이미지 폭 (모바일) | `w-full` = 100% | |
| 이미지 폭 (데스크톱) | `lg:w-4/5` = 80% | 좌우 여백으로 시선 집중 |
| 이미지 높이 | `h-96` = 384px (고정) | object-cover로 비율 유지 |
| Nav 전환 | `lg` (1024px) | 레이아웃 브레이크포인트와 일치 |

### 핵심 결정

- **레이아웃 전환 없음** — 모바일·데스크톱 모두 동일한 단일 컬럼. 반응형 복잡도 최소화
- **텍스트 폭 제한 + 중앙 정렬** — `max-w-lg mx-auto`로 넓은 화면에서도 라인 길이 제어
- **이미지 고정 높이** — `h-96 object-cover`로 이미지 비율과 무관하게 일관된 시각 무게 유지
- **이미지를 데스크톱에서 80%로 축소** — 살짝 좁혀 "프레임 안에 있는 느낌" 연출
- **Nav도 lg에서 전환** — Pattern 01(nav=md, layout=lg)과 달리 동일 브레이크포인트 → 태블릿에서 햄버거 유지

### 코드 골격

```html
<div class="container px-6 py-16 mx-auto text-center">
  <div class="max-w-lg mx-auto">
    <h1>...</h1>
    <p class="mt-6">...</p>
    <button class="mt-6">CTA</button>
    <p class="mt-3 text-sm">sub-text</p>
  </div>
  <div class="flex justify-center mt-10">
    <img class="object-cover w-full h-96 rounded-xl lg:w-4/5" src="..." />
  </div>
</div>
```

### Pattern 01과 비교

| 항목 | Pattern 01 (Split) | Pattern 02 (Centered) |
|------|-------------------|-----------------------|
| 레이아웃 구조 | 좌우 분할 (6:6) | 단일 컬럼 |
| 텍스트 정렬 | 좌측 | 중앙 |
| 이미지 위치 | 텍스트 우측 | 텍스트 아래 |
| 반응형 복잡도 | 높음 (flex 전환) | 낮음 (전환 없음) |
| Nav 브레이크포인트 | md (768px) | lg (1024px) |

### 사용 맥락

- SaaS 툴, 앱 랜딩 (제품 스크린샷을 크게 보여줄 때)
- 메시지가 명확하고 짧을 때 (H1 1~2줄)
- 이미지가 UI 스크린샷·대시보드처럼 가로로 긴 경우

---

## Pattern 03 — Full-Bleed Split (이미지가 뷰포트 끝까지, 컨테이너 없음)

### 레이아웃 구조

```
Mobile (< 1024px)          Desktop (≥ 1024px / lg)
┌───────────────────┐      ┌──────────────┬──────────────┐
│  텍스트 (px-6)    │      │  텍스트       │  이미지       │
│  py-8, max-w-xl   │      │  w-1/2       │  w-1/2       │
│  CTA 2버튼 (세로)  │      │  h-[32rem]   │  h-auto      │
├───────────────────┤      │  CTA 2버튼   │  bg-cover    │
│  이미지 (h-64)    │      │  (가로)      │  + 오버레이   │
│  bg-cover         │      └──────────────┴──────────────┘
└───────────────────┘
```

### 수치

| 항목 | 값 | 비고 |
|------|-----|------|
| Container | **없음** — `lg:flex`가 직접 뷰포트 폭 점유 | Pattern 01·02와 핵심 차이 |
| 텍스트 수평 패딩 | `px-6` = 24px | 컨테이너 대신 직접 지정 |
| 텍스트 수직 패딩 | `py-8` = 32px | Pattern 01(py-16=64px)의 절반 |
| 텍스트 최대 폭 | `max-w-xl` = 576px | |
| 텍스트 영역 고정 높이 | `lg:h-[32rem]` = 512px | 이미지와 높이 맞춤 |
| 이미지 폭 | `lg:w-1/2` = 50% | 모바일은 `w-full h-64` |
| 이미지 모바일 높이 | `h-64` = 256px (고정) | |
| 이미지 처리 | `bg-cover` + `bg-black opacity-25` 오버레이 | `<img>` 아닌 배경 이미지 |
| Nav | `px-6 py-4 shadow` — container 없음 | 전체 폭 nav |
| Nav 전환 | `lg` (1024px) | |
| CTA 버튼 배치 | 모바일 세로(`flex-col`) → 데스크톱 가로(`lg:flex-row`) | |

### 핵심 결정

- **container 제거** — 이미지가 화면 끝까지 출혈(full-bleed). 콘텐츠 폭 제한 없이 시각적 임팩트 극대화
- **이미지를 `<img>`가 아닌 `background-image`로** — 비율 왜곡 없이 영역을 채우는 bg-cover 활용
- **다크 오버레이** — `bg-black opacity-25`로 이미지 위 텍스트 가독성 확보 (이 예시에서는 텍스트가 이미지 위에 없지만, 패턴 확장 시 활용 가능)
- **텍스트 영역 고정 높이** — `lg:h-[32rem]`으로 이미지와 높이를 맞춰 수평으로 정렬
- **CTA 방향 전환** — 모바일 세로 → 데스크톱 가로. `space-y-3 lg:space-y-0 lg:flex-row`

### 코드 골격

```html
<div class="lg:flex">
  <!-- 텍스트: 고정 높이 + 내부 중앙 정렬 -->
  <div class="flex items-center justify-center w-full px-6 py-8 lg:h-[32rem] lg:w-1/2">
    <div class="max-w-xl">
      <h2>...</h2>
      <p class="mt-4">...</p>
      <div class="flex flex-col mt-6 space-y-3 lg:space-y-0 lg:flex-row">
        <a>Primary CTA</a>
        <a class="lg:mx-4">Secondary CTA</a>
      </div>
    </div>
  </div>

  <!-- 이미지: 배경 이미지 + 오버레이 -->
  <div class="w-full h-64 lg:w-1/2 lg:h-auto">
    <div class="w-full h-full bg-cover" style="background-image: url(...)">
      <div class="w-full h-full bg-black opacity-25"></div>
    </div>
  </div>
</div>
```

### 3패턴 비교

| 항목 | P01 Split | P02 Centered | P03 Full-Bleed |
|------|-----------|--------------|----------------|
| Container | O | O | **X** |
| 이미지 처리 | `<img>` | `<img>` | **bg-image** |
| 이미지 위치 | 우측 (inline) | 하단 | 우측 (full-bleed) |
| 텍스트 정렬 | 좌 | 중앙 | 좌 |
| 시각 임팩트 | 중 | 중 | **높음** |
| 반응형 복잡도 | 중 | 낮음 | 중 |

### 사용 맥락

- 사진 중심 브랜드 (패션, 호텔, 에이전시)
- 히어로에서 강한 시각적 존재감이 필요할 때
- 이미지가 일러스트보다 실제 사진일 때

---
