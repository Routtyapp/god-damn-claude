# CSS tan()

## 개념

```
tan(각도) = sin(각도) / cos(각도)
           = 직각삼각형의 대변 / 인접변
```

단위원에서 tan()은 중심에서 출발한 직선이 X=1 수직선과 만나는 점까지의 거리다.

### 주의

| 각도 | 결과 |
|------|------|
| 0° | 0 |
| 45° | 1 |
| 90° | ∞ (정의 불가, 매우 큰 수 반환) |
| 135° | -1 |
| 180° | 0 |

**90°와 270°는 사용 금지.**

---

## 1. 다각형 섹션 분할

원을 N개의 삼각형 조각으로 간격 없이 분할한다.

### 원리

원을 N등분할 때 각 조각의 절반 각도(θ)와 반지름(r)으로
삼각형 높이를 계산한다.

```
θ = 360deg / 2 / N      ← 각 조각의 절반 각도
높이 = 2 * r * tan(θ)   ← 조각의 전체 높이
```

### HTML

```html
<ul style="--total: 6">
  <li style="--i: 1">1</li>
  <li style="--i: 2">2</li>
  <li style="--i: 3">3</li>
  <!-- ... -->
</ul>
```

### CSS

```css
ul {
  --radius: 200px;
  position: relative;
  width: calc(var(--radius) * 2);
  height: calc(var(--radius) * 2);
  border-radius: 50%;
  overflow: hidden;
}

li {
  --theta: calc(360deg / 2 / var(--total));
  --rotation: calc(360deg / var(--total) * var(--i));

  position: absolute;
  top: 0;
  right: 0;
  width: var(--radius);

  /* tan()으로 삼각형 높이 계산 — 간격 없이 완벽하게 맞아떨어짐 */
  height: calc(2 * var(--radius) * tan(var(--theta)));

  /* 삼각형으로 클리핑 */
  clip-path: polygon(100% 0, 0 50%, 100% 100%);

  /* 원의 중심을 기준으로 회전 */
  transform-origin: left center;
  transform: rotate(var(--rotation));
}
```

### 작동 원리

1. 각 `li`를 직각삼각형(`clip-path: polygon`)으로 만든다
2. `height`를 `tan(θ)`으로 계산해 조각이 정확히 맞아떨어지게 한다
3. 원의 중심(왼쪽 가운데)을 기준으로 회전시킨다

---

## 2. 원형 메뉴 (GTA 스타일)

다각형 분할을 인터랙티브 방사형 메뉴로 확장한다.

```html
<ul class="radial-menu" style="--total: 8">
  <li style="--i: 1"><button>아이템 1</button></li>
  <li style="--i: 2"><button>아이템 2</button></li>
  <!-- ... -->
</ul>
```

```css
.radial-menu {
  --radius: 180px;
  position: relative;
  width: calc(var(--radius) * 2);
  height: calc(var(--radius) * 2);
  border-radius: 50%;
  overflow: hidden;
}

.radial-menu li {
  --theta: calc(360deg / 2 / var(--total));
  --rotation: calc(360deg / var(--total) * var(--i));

  position: absolute;
  top: 0;
  right: 0;
  width: var(--radius);
  height: calc(2 * var(--radius) * tan(var(--theta)));

  clip-path: polygon(100% 0, 0 50%, 100% 100%);
  transform-origin: left center;
  transform: rotate(var(--rotation));

  transition: background-color 0.2s;
}

.radial-menu li:hover {
  background-color: oklch(70% 0.15 250);
}

/* 버튼 텍스트 위치 조정 */
.radial-menu li button {
  position: absolute;
  right: 1.5rem;
  top: 50%;
  transform: translateY(-50%) rotate(calc(-1 * var(--rotation)));
}
```

---

## 3. 다각형 이미지 갤러리

이미지를 파이 조각처럼 배열한다.

```css
.gallery {
  --radius: 250px;
  --total: 5;
  position: relative;
  width: calc(var(--radius) * 2);
  height: calc(var(--radius) * 2);
  border-radius: 50%;
  overflow: hidden;
}

.gallery li {
  --theta: calc(360deg / 2 / var(--total));
  --rotation: calc(360deg / var(--total) * var(--i));

  position: absolute;
  top: 0;
  right: 0;
  width: var(--radius);
  height: calc(2 * var(--radius) * tan(var(--theta)));

  clip-path: polygon(100% 0, 0 50%, 100% 100%);
  transform-origin: left center;
  transform: rotate(var(--rotation));
  overflow: hidden;
}

.gallery li img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  /* 회전 보정 */
  transform: rotate(calc(-1 * var(--rotation)));
  transform-origin: left center;
}
```

---

## cos/sin과 tan의 선택 기준

| 목적 | 선택 | 이유 |
|------|------|------|
| 원 위에 점(요소) 배치 | `cos` + `sin` | 좌표(x, y) 직접 계산 |
| 원을 조각으로 분할 | `tan` | 삼각형 변의 비율 계산 |
| 파동/나선 움직임 | `cos` + `sin` | 진동 특성 활용 |
| 대각선 섹션 높이 | `tan` | 경사각 → 오프셋 거리 변환 |

---

## 실전 팁

```css
/* ✅ CSS 변수 + calc() 조합으로 마법 숫자 제거 */
--theta: calc(360deg / 2 / var(--total));
height: calc(2 * var(--radius) * tan(var(--theta)));

/* ❌ 하드코딩 — N이 바뀌면 모두 재계산해야 함 */
height: 207px;

/* ✅ --total 하나만 바꾸면 전체 레이아웃이 자동 재계산됨 */
```

`tan()`의 진짜 가치는 **"N을 바꾸면 전체가 자동으로 재계산"** 되는 유지보수성이다.
