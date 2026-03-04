# 파이 차트 공식 가이드

## 기본 사용

```python
import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']

plt.pie(ratio, labels=labels, autopct='%.1f%%')
plt.show()
```

## 시작 각도와 방향

```python
plt.pie(ratio, labels=labels, autopct='%.1f%%',
        startangle=260,       # 시작 각도 (0 = 양의 x축)
        counterclock=False)   # True(기본)=반시계, False=시계 방향
```

## 조각 분리 (explode)

```python
explode = [0, 0.10, 0, 0.10]   # 반지름 대비 분리 비율

plt.pie(ratio, labels=labels, autopct='%.1f%%',
        startangle=260, counterclock=False,
        explode=explode)
```

## 그림자

```python
explode = [0.05, 0.05, 0.05, 0.05]

plt.pie(ratio, labels=labels, autopct='%.1f%%',
        startangle=260, counterclock=False,
        explode=explode, shadow=True)
```

## 색상 지정

```python
# 색상 이름
colors = ['silver', 'gold', 'whitesmoke', 'lightgray']

# Hex 코드
colors = ['#ff9999', '#ffc000', '#8fd9b6', '#d395d0']

plt.pie(ratio, labels=labels, autopct='%.1f%%',
        colors=colors, explode=explode, shadow=True)
```

## 도넛 차트 (wedgeprops)

```python
wedgeprops = {
    'width': 0.7,          # 두께 비율 (1.0=파이, <1.0=도넛)
    'edgecolor': 'w',      # 조각 경계 색
    'linewidth': 5         # 경계 두께
}

plt.pie(ratio, labels=labels, autopct='%.1f%%',
        colors=colors, wedgeprops=wedgeprops)
```

## 파라미터 요약

| 파라미터 | 설명 | 기본값 |
|----------|------|--------|
| `autopct` | 숫자 형식 (`'%.1f%%'`) | `None` |
| `startangle` | 시작 각도 (도) | `0` |
| `counterclock` | 반시계 방향 여부 | `True` |
| `explode` | 조각 분리 비율 리스트 | `None` |
| `shadow` | 그림자 표시 | `False` |
| `colors` | 조각 색상 리스트 | 기본 팔레트 |
| `wedgeprops` | 조각 스타일 딕셔너리 | `None` |
| `pctdistance` | 퍼센트 텍스트 위치 (반지름 비율) | `0.6` |
| `labeldistance` | 레이블 위치 | `1.1` |
