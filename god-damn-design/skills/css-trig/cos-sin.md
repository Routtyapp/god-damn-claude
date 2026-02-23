# CSS cos() / sin()

## 개념

단위원(반지름 = 1) 위의 점을 각도로 추적한다.

```
cos(각도) → X 좌표
sin(각도) → Y 좌표
```

반지름 `r`을 곱하면 실제 픽셀 거리가 된다.

```
x = cos(각도) * r
y = sin(각도) * r
```

---

## 1. 원형 레이아웃

요소 N개를 원 주위에 균등 배치한다.

### HTML

```html
<ul style="--total: 9">
  <li style="--i: 0">0</li>
  <li style="--i: 1">1</li>
  <li style="--i: 2">2</li>
  <!-- ... -->
</ul>
```

### CSS

```css
ul {
  --radius: 10rem;
  position: relative;
}

li {
  /* 360도를 N등분하고 인덱스를 곱해 각 요소의 각도를 결정 */
  --rotation: calc(360deg / var(--total) * var(--i));

  position: absolute;
  transform:
    translateX(calc(cos(var(--rotation)) * var(--radius)))
    translateY(calc(sin(var(--rotation)) * var(--radius)));
}
```

### 반원형 메뉴 변형

```css
li {
  /* 180도만 사용 */
  --rotation: calc(180deg / var(--total) * var(--i));

  transform:
    translateX(calc(cos(var(--rotation)) * var(--radius)))
    translateY(calc(sin(var(--rotation)) * var(--radius)));
}
```

### React 컴포넌트

```tsx
interface CircularLayoutProps {
  items: React.ReactNode[];
  radius?: number; // rem
  startAngle?: number; // deg, 기본 0
  spread?: number; // deg, 기본 360 (전체 원)
}

const CircularLayout: React.FC<CircularLayoutProps> = ({
  items,
  radius = 10,
  startAngle = 0,
  spread = 360,
}) => {
  const total = items.length;

  return (
    <div style={{ position: 'relative', width: 0, height: 0 }}>
      {items.map((item, i) => {
        const angleDeg = startAngle + (spread / total) * i;
        const angleRad = (angleDeg * Math.PI) / 180;
        const x = Math.cos(angleRad) * radius;
        const y = Math.sin(angleRad) * radius;

        return (
          <div
            key={i}
            style={{
              position: 'absolute',
              transform: `translate(${x}rem, ${y}rem)`,
            }}
          >
            {item}
          </div>
        );
      })}
    </div>
  );
};
```

---

## 2. 파동(Wave) 레이아웃

`sin()`의 진동 특성으로 요소를 물결 형태로 배치한다.

### 기본 파동

```css
ul {
  --shape-size: 4rem;
}

li {
  /* 60deg 간격으로 sin 파동 생성 */
  transform: translateY(
    calc(sin(60deg * var(--i)) * var(--shape-size) / 2)
  );
}
```

### DNA 나선 효과

두 줄의 파동을 90도 오프셋으로 교차시킨다.

```html
<ul class="strand-a" style="--total: 8">
  <li style="--i: 0"></li>
  <!-- ... -->
</ul>
<ul class="strand-b" style="--total: 8">
  <li style="--i: 0"></li>
  <!-- ... -->
</ul>
```

```css
.strand-a li {
  transform: translateY(
    calc(sin(60deg * var(--i)) * var(--shape-size) / 2)
  );
}

.strand-b li {
  /* cos()으로 90도 위상 차이 생성 */
  transform: translateY(
    calc(cos(60deg * var(--i) + 60deg) * var(--shape-size) / 2)
  );
}
```

---

## 3. 감쇠 진동 애니메이션 (Damped Oscillation)

물리 기반 "스프링처럼 튀다 멈추는" 움직임이다.

### 수학 공식

```
위치 = e^(-γt) × A × cos(ωt - α)

e^(-γt)  → 지수 감쇠 (시간이 지날수록 진폭이 줄어듦)
A        → 초기 진폭
cos(ωt)  → 진동 패턴
```

### CSS 구현 (1축 — 좌우 진동)

```css
/* @property로 숫자형 변수 선언 (애니메이션 보간을 위해 필수) */
@property --progress {
  syntax: "<number>";
  initial-value: 0;
  inherits: true;
}

:root {
  --amplitude: 200px;
  --damping: 0.3;
  --frequency: 0.8;
  --offset: calc(pi / 2);
}

@keyframes movement {
  from { --progress: 0; }
  to   { --progress: 25; }
}

.ball {
  --oscillation: calc(
    exp(-1 * var(--damping) * var(--progress))
    * var(--amplitude)
    * cos(var(--frequency) * var(--progress) - var(--offset))
  );

  transform: translateX(var(--oscillation));
  animation: movement 2s linear forwards;
}
```

### CSS 구현 (2축 — 나선형 감쇠)

```css
.ball {
  --oscillation-x: calc(
    exp(-1 * var(--damping) * var(--progress))
    * var(--amplitude)
    * cos(var(--frequency) * var(--progress) - var(--offset))
  );
  --oscillation-y: calc(
    exp(-1 * var(--damping) * var(--progress))
    * var(--amplitude)
    * sin(var(--frequency) * var(--progress) - var(--offset))
  );

  transform:
    translateX(var(--oscillation-x))
    translateY(var(--oscillation-y));

  animation: movement 2s linear forwards;
}
```

### 주요 파라미터

| 변수 | 역할 | 값이 클수록 |
|------|------|------------|
| `--amplitude` | 초기 진폭 (최대 이동 거리) | 더 크게 흔들림 |
| `--damping` | 감쇠 계수 γ | 빠르게 멈춤 |
| `--frequency` | 진동 주파수 ω | 더 빠르게 진동 |
| `--offset` | 위상 오프셋 α | 시작 위치 조정 |

---

## 조합 패턴

| 패턴 | 사용 함수 | 핵심 기법 |
|------|----------|----------|
| 원형 메뉴 | `cos` + `sin` | 360deg / N × i |
| 반원 메뉴 | `cos` + `sin` | 180deg / N × i |
| 파동 리스트 | `sin` | 고정 각도 × i |
| DNA 나선 | `sin` + `cos` | 90도 위상 차 |
| 스프링 애니메이션 | `cos` | exp() × cos() |
| 나선 감쇠 | `cos` + `sin` | 2축 exp() 감쇠 |
