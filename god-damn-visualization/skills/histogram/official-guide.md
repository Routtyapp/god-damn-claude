# 히스토그램 공식 가이드

## 기본 사용

```python
import matplotlib.pyplot as plt

weight = [68, 81, 64, 56, 78, 74, 61, 77, 66, 68, 59, 71,
          80, 59, 67, 81, 69, 73, 69, 74, 70, 65]

plt.hist(weight)
plt.show()
```

## 구간 개수 지정 (bins)

`bins` 수에 따라 분포 모양이 달라지므로 적절히 선택:

```python
plt.hist(weight, label='bins=10')        # 기본값
plt.hist(weight, bins=30, label='bins=30')
plt.legend()
plt.show()
```

## 누적 히스토그램

```python
plt.hist(weight, cumulative=True,  label='누적')
plt.hist(weight, cumulative=False, label='일반')
plt.legend(loc='upper left')
plt.show()
```

## histtype 종류

```python
weight2 = [52, 67, 84, 66, 58, 78, 71, 57, 76, 62, 51, 79,
           69, 64, 76, 57, 63, 53, 79, 64, 50, 61]

plt.hist((weight, weight2), histtype='bar')        # 기본, 나란히
plt.hist((weight, weight2), histtype='barstacked') # 누적 막대
plt.hist((weight, weight2), histtype='step')       # 윤곽선만
plt.hist((weight, weight2), histtype='stepfilled') # 채운 윤곽선
```

## 밀도 함수 (density) + 다중 분포

```python
import matplotlib.pyplot as plt
import numpy as np

a = 2.0 * np.random.randn(10000) + 1.0   # 표준편차 2, 평균 1 정규분포
b = np.random.standard_normal(10000)      # 표준정규분포
c = 20.0 * np.random.rand(5000) - 10.0   # 균일분포

plt.hist(a, bins=100, density=True, alpha=0.7, histtype='step')
plt.hist(b, bins=50,  density=True, alpha=0.5, histtype='stepfilled')
plt.hist(c, bins=100, density=True, alpha=0.9, histtype='step')
plt.show()
```

`density=True`: 막대 아래 면적 합 = 1 (확률 밀도 함수)

## 파라미터 요약

| 파라미터 | 설명 | 기본값 |
|----------|------|--------|
| `bins` | 구간 수 또는 경계 배열 | `10` |
| `density` | 밀도 함수 여부 | `False` |
| `cumulative` | 누적 여부 | `False` |
| `histtype` | `'bar'`/`'barstacked'`/`'step'`/`'stepfilled'` | `'bar'` |
| `color` | 색상 | `None` |
| `alpha` | 투명도 | `None` |
| `edgecolor` | 막대 테두리 색 | `None` |
| `label` | 범례 레이블 | `None` |
| `orientation` | `'vertical'`/`'horizontal'` | `'vertical'` |
