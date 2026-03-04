# 히트맵 공식 가이드

## 기본 사용 (matshow)

```python
import matplotlib.pyplot as plt
import numpy as np

arr = np.random.standard_normal((30, 40))

plt.matshow(arr)
plt.show()
```

## 컬러바 추가

```python
plt.matshow(arr)
plt.colorbar()
plt.show()

# 크기 조절
plt.colorbar(shrink=0.8, aspect=10)
# shrink: 컬러바 길이 비율 (기본 1.0)
# aspect: 종횡비 (기본 20)
```

## 색상 범위 지정 (clim)

```python
plt.matshow(arr)
plt.colorbar(shrink=0.8, aspect=10)
plt.clim(-3.0, 3.0)   # 범위 밖 값은 경계 색으로 클리핑
plt.show()
```

## 컬러맵 지정

```python
cmap = plt.get_cmap('bwr')    # Blue-White-Red 분기형
# cmap = plt.get_cmap('PiYG')
# cmap = plt.get_cmap('BuGn')
# cmap = plt.get_cmap('Greys')

plt.matshow(arr, cmap=cmap)
plt.colorbar()
plt.show()
```

## imshow vs matshow

| 함수 | 특징 |
|------|------|
| `matshow()` | 행렬 시각화 전용, axes가 위쪽에 표시 |
| `imshow()` | 이미지·행렬 모두, aspect 제어 가능 |

```python
# imshow로 히트맵 + 셀 값 표시
import seaborn as sns  # 또는 순수 matplotlib 사용

fig, ax = plt.subplots()
im = ax.imshow(arr, cmap='coolwarm')
plt.colorbar(im)

# 셀 값 텍스트 표시
for i in range(arr.shape[0]):
    for j in range(arr.shape[1]):
        ax.text(j, i, f'{arr[i,j]:.1f}',
                ha='center', va='center', fontsize=6)
plt.show()
```

## 자주 쓰는 컬러맵

| cmap | 적합한 데이터 |
|------|--------------|
| `'viridis'` | 단방향 (밀도, 강도) |
| `'coolwarm'` | 분기형 (상관계수, 온도 편차) |
| `'RdYlGn'` | 빨강-노랑-초록 (점수, 위험도) |
| `'Blues'` | 단색 강도 |
| `'bwr'` | 파랑-흰색-빨강 분기형 |
