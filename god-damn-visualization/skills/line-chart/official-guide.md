# 라인 차트 & 공통 설정 공식 가이드

## 목차
1. [축 레이블](#축-레이블)
2. [범례](#범례)
3. [축 범위](#축-범위)
4. [선 스타일](#선-스타일) → 전체 참조는 [style-guide/official-guide.md](../style-guide/official-guide.md)

---

## 축 레이블

```python
import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.xlabel('X-Label')
plt.ylabel('Y-Label')
```

### 여백·폰트·위치

```python
plt.xlabel('X-Axis', labelpad=15)           # 레이블 여백
plt.ylabel('Y-Axis', labelpad=20)

# 폰트 커스터마이즈
font1 = {'family': 'serif', 'color': 'b', 'weight': 'bold', 'size': 14}
plt.xlabel('X-Axis', labelpad=15, fontdict=font1)

# 위치 (Matplotlib 3.3+)
plt.xlabel('X-Axis', loc='right')   # 'left' / 'center' / 'right'
plt.ylabel('Y-Axis', loc='top')     # 'bottom' / 'center' / 'top'
```

---

## 범례

```python
plt.plot(x, y, label='Series A')
plt.legend()
```

### 위치 (loc)

```python
plt.legend(loc='best')          # 자동 최적 위치 (기본값)
plt.legend(loc='upper right')
plt.legend(loc='lower left')
plt.legend(loc=(0.5, 0.5))     # (x, y) 축 비율로 직접 지정
```

### loc 문자열 옵션

`'best'` · `'upper right'` · `'upper left'` · `'lower left'` · `'lower right'`
`'right'` · `'center left'` · `'center right'` · `'lower center'` · `'upper center'` · `'center'`

### 추가 스타일

```python
plt.legend(
    loc='best',
    ncol=2,         # 열 개수
    fontsize=14,    # 폰트 크기
    frameon=True,   # 테두리 표시
    shadow=True,    # 그림자
    facecolor='lightyellow',
    edgecolor='gray'
)
```

---

## 축 범위

```python
# 개별 지정
plt.xlim(0, 10)
plt.ylim(-5, 5)

# 한번에
plt.axis([xmin, xmax, ymin, ymax])

# 특수 모드
plt.axis('equal')   # x/y 스케일 동일
plt.axis('square')  # 정사각형 영역
plt.axis('tight')   # 데이터에 딱 맞게
plt.axis('off')     # 축 숨기기
```

---

## 선 스타일

```python
plt.plot(x, y,
         color='steelblue',
         linestyle='--',    # '-' / '--' / ':' / '-.'
         linewidth=2,
         marker='o',
         markersize=8,
         alpha=0.8,
         label='Series')
```

전체 선 종류·마커·색상 참조 → [style-guide/official-guide.md](../style-guide/official-guide.md)
