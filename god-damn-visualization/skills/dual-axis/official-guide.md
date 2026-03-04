# 이중 축 공식 가이드

## 기본 사용 (twinx)

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 3)
y1 = x + 1
y2 = -x - 1

fig, ax1 = plt.subplots()
ax1.plot(x, y1, color='green')

ax2 = ax1.twinx()   # x축을 공유하는 두 번째 y축
ax2.plot(x, y2, color='deeppink')

plt.show()
```

## 축 레이블

```python
fig, ax1 = plt.subplots()
ax1.set_xlabel('X-Axis')
ax1.set_ylabel('1st Y-Axis')
ax1.plot(x, y1, color='green')

ax2 = ax1.twinx()
ax2.set_ylabel('2nd Y-Axis')
ax2.plot(x, y2, color='deeppink')
```

## 범례 (각 축 별도)

```python
ax1.plot(x, y1, color='green', label='1st Data')
ax1.legend(loc='upper right')

ax2.plot(x, y2, color='deeppink', label='2nd Data')
ax2.legend(loc='lower right')
```

## 범례 하나로 합치기

```python
line1 = ax1.plot(x, y1, color='green', label='1st Data')
line2 = ax2.plot(x, y2, color='deeppink', label='2nd Data')

lines = line1 + line2
labels = [l.get_label() for l in lines]
ax1.legend(lines, labels, loc='upper right')
```

## 라인 + 바 복합 차트

```python
import matplotlib.pyplot as plt
import numpy as np

plt.style.use('default')
plt.rcParams['figure.figsize'] = (4, 3)
plt.rcParams['font.size'] = 12

x = np.arange(2020, 2027)
y1 = np.array([1, 3, 7, 5, 9, 7, 14])   # 라인 데이터
y2 = np.array([1, 3, 5, 7, 9, 11, 13])  # 바 데이터

fig, ax1 = plt.subplots()

ax1.plot(x, y1, '-s', color='green', markersize=7, linewidth=5, alpha=0.7, label='Price')
ax1.set_ylim(0, 18)
ax1.set_xlabel('Year')
ax1.set_ylabel('Price ($)')
ax1.tick_params(axis='both', direction='in')

ax2 = ax1.twinx()
ax2.bar(x, y2, color='deeppink', label='Demand', alpha=0.7, width=0.7)
ax2.set_ylim(0, 18)
ax2.set_ylabel(r'Demand ($\times10^6$)')
ax2.tick_params(axis='y', direction='in')

plt.show()
```

## zorder (레이어 순서 제어)

라인이 바 뒤에 가려지는 문제 해결:

```python
ax1.set_zorder(ax2.get_zorder() + 10)   # ax1을 앞으로
ax1.patch.set_visible(False)             # ax1 배경 투명화

ax1.legend(loc='upper left')
ax2.legend(loc='upper right')
```

## twiny (y축 공유, x축 이중)

```python
ax2 = ax1.twiny()   # y축 공유, x축 두 개
```
