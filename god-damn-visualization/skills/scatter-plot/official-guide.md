# 산점도 공식 가이드

## 기본 사용

```python
import matplotlib.pyplot as plt
import numpy as np

np.random.seed(0)
n = 50
x = np.random.rand(n)
y = np.random.rand(n)

plt.scatter(x, y)
plt.show()
```

## 색상과 크기 지정

```python
area = (30 * np.random.rand(n))**2   # 마커 크기는 size**2 형태
colors = np.random.rand(n)            # 0~1 범위 숫자 시퀀스

plt.scatter(x, y, s=area, c=colors)
plt.show()
```

> `scatter(s=20**2)`와 `plot(markersize=20)`은 같은 크기:
> ```python
> plt.plot([1], [1], 'o', markersize=20, c='#FF5733')
> plt.scatter([2], [1], s=20**2, c='#33FFCE')
> ```

## 투명도와 컬러맵

```python
plt.scatter(x, y, s=area, c=colors,
            alpha=0.5,          # 투명도 0~1
            cmap='Spectral')    # 컬러맵
plt.colorbar()
plt.show()
```

## 자주 쓰는 컬러맵

| cmap | 특징 |
|------|------|
| `'viridis'` | 지각균일, 기본 권장 |
| `'plasma'` | 고대비 |
| `'Spectral'` | 분기형, 중간 강조 |
| `'coolwarm'` | 파랑-빨강 분기 |
| `'Greys'` | 단색 |

## 파라미터 요약

| 파라미터 | 설명 | 기본값 |
|----------|------|--------|
| `s` | 마커 크기 (`size**2`) | `rcParams['lines.markersize']**2` |
| `c` | 색상 (단일/시퀀스/rgb/hex) | `None` |
| `alpha` | 투명도 | `None` |
| `cmap` | 컬러맵 이름 | `None` |
| `marker` | 마커 모양 | `'o'` |
| `edgecolors` | 마커 테두리 색 | `'face'` |
| `linewidths` | 마커 테두리 두께 | `1.5` |
