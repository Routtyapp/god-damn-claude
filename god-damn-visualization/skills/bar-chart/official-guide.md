# 막대 차트 공식 가이드

## 기본 막대 그래프

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.bar(x, values)
plt.xticks(x, years)
plt.show()
```

## 색상 지정

```python
# 단일 색상
plt.bar(x, values, color='dodgerblue')
# plt.bar(x, values, color='y')
# plt.bar(x, values, color='C2')
# plt.bar(x, values, color='#e35f62')

# 막대마다 다른 색상
colors = ['y', 'dodgerblue', 'C2']
plt.bar(x, values, color=colors)
```

## 막대 폭 지정

```python
plt.bar(x, values, width=0.4)  # 기본값 0.8
```

## 스타일 (테두리, 정렬)

```python
plt.bar(x, values,
        align='edge',          # 'center'(기본) 또는 'edge'
        edgecolor='lightgray',
        linewidth=5,
        tick_label=years)
```

## 수평 막대 그래프 (barh)

```python
import matplotlib.pyplot as plt
import numpy as np

y = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.barh(y, values)
plt.yticks(y, years)
plt.show()
```

### barh 색상/높이

```python
# 색상
plt.barh(y, values, color='dodgerblue')

# 높이 (barh에서 height = 막대 두께)
plt.barh(y, values, height=0.4)  # 기본값 0.8

# 스타일
plt.barh(y, values,
         align='edge',
         edgecolor='#eee',
         linewidth=5,
         tick_label=years)
```

## 파라미터 요약

| 파라미터 | 설명 | 기본값 |
|----------|------|--------|
| `color` | 막대 색상 (단일/리스트) | `'C0'` |
| `width` | 막대 폭 (bar) | `0.8` |
| `height` | 막대 두께 (barh) | `0.8` |
| `align` | 눈금 기준 정렬 | `'center'` |
| `edgecolor` | 테두리 색 | `None` |
| `linewidth` | 테두리 두께 | `1.0` |
| `tick_label` | 틱 레이블 문자열 | `None` |
| `alpha` | 투명도 | `1.0` |
