# 스타일 공식 가이드

## 목차
1. [스타일 테마](#스타일-테마)
2. [rcParams](#rcparams)
3. [선 종류 (linestyle)](#선-종류)
4. [마커 (marker)](#마커)
5. [색상 지정](#색상-지정)
6. [이미지 저장 (savefig)](#이미지-저장)

---

## 스타일 테마

```python
import matplotlib.pyplot as plt

plt.style.use('bmh')
# plt.style.use('ggplot')
# plt.style.use('classic')
# plt.style.use('Solarize_Light2')
# plt.style.use('dark_background')
# plt.style.use('default')

# 사용 가능한 스타일 목록
print(plt.style.available)
```

---

## rcParams

```python
plt.style.use('default')

# 그래프 크기 / 폰트
plt.rcParams['figure.figsize'] = (6, 3)
plt.rcParams['font.size'] = 12

# 선 스타일
plt.rcParams['lines.linewidth'] = 3
plt.rcParams['lines.linestyle'] = '-'

# 눈금 방향
plt.rcParams['xtick.top'] = True
plt.rcParams['ytick.right'] = True
plt.rcParams['xtick.direction'] = 'in'    # 'in' / 'out' / 'inout'
plt.rcParams['ytick.direction'] = 'in'
plt.rcParams['xtick.major.size'] = 7
plt.rcParams['xtick.minor.visible'] = True

# 한글 폰트 (Windows)
plt.rcParams['font.family'] = 'Malgun Gothic'
plt.rcParams['axes.unicode_minus'] = False
```

---

## 선 종류

### 기본 포맷 문자열

```python
plt.plot(x, y, '-',  label='Solid')
plt.plot(x, y, '--', label='Dashed')
plt.plot(x, y, ':',  label='Dotted')
plt.plot(x, y, '-.', label='Dash-dot')
```

### linestyle 파라미터

```python
plt.plot(x, y, linestyle='solid')
plt.plot(x, y, linestyle='dashed')
plt.plot(x, y, linestyle='dotted')
plt.plot(x, y, linestyle='dashdot')
```

### 튜플로 커스터마이즈 `(offset, (on, off, ...))`

```python
plt.plot(x, y, linestyle=(0, (1, 1)))      # dotted와 동일
plt.plot(x, y, linestyle=(0, (5, 5)))      # dashed와 동일
plt.plot(x, y, linestyle=(0, (3, 5, 1, 5))) # dashdotted
```

### Named Linestyles (파라미터 튜플)

| 이름 | 튜플 |
|------|------|
| loosely dotted | `(0, (1, 10))` |
| dotted | `(0, (1, 1))` |
| loosely dashed | `(0, (5, 10))` |
| dashed | `(0, (5, 5))` |
| densely dashed | `(0, (5, 1))` |
| loosely dashdotted | `(0, (3, 10, 1, 10))` |
| dashdotted | `(0, (3, 1, 1, 1))` |
| densely dashdotdotted | `(0, (3, 1, 1, 1, 1, 1))` |

---

## 마커

### 포맷 문자열

```python
plt.plot(x, y, 'bo')   # 파란 원
plt.plot(x, y, 'r^')   # 빨간 삼각형 위
plt.plot(x, y, 'g-')   # 초록 실선
plt.plot(x, y, 'k^:')  # 검정 삼각형, 점선
plt.plot(x, y, 'bo--') # 파란 원 + 점선
```

### Colors (포맷 문자)

| 문자 | 색상 | 문자 | 색상 |
|------|------|------|------|
| `'b'` | blue | `'c'` | cyan |
| `'g'` | green | `'m'` | magenta |
| `'r'` | red | `'y'` | yellow |
| `'k'` | black | `'w'` | white |

### 주요 마커

| 문자 | 모양 | 문자 | 모양 |
|------|------|------|------|
| `'.'` | point | `'o'` | circle |
| `'s'` | square | `'^'` | triangle up |
| `'v'` | triangle down | `'D'` | diamond |
| `'*'` | star | `'+'` | plus |
| `'x'` | x | `'p'` | pentagon |
| `'h'` | hexagon1 | `'H'` | hexagon2 |

### marker 파라미터

```python
plt.plot(x, y, marker='H')
plt.plot(x, y, marker='d')
plt.plot(x, y, marker=11)       # CARETDOWNBASE
plt.plot(x, y, marker='$Z$')    # 수학 기호
```

---

## 색상 지정

```python
# 포맷 문자
plt.plot(x, y, 'r')

# color 파라미터 (이름, C숫자, hex, RGB)
plt.plot(x, y, color='steelblue')
plt.plot(x, y, color='C0')           # 기본 색상 사이클 인덱스
plt.plot(x, y, color='#FF5733')      # Hex
plt.plot(x, y, color=(0.1, 0.2, 0.5)) # RGB 0~1
```

---

## 이미지 저장

```python
# 기본 저장
plt.savefig('output.png')

# 고해상도 저장
plt.savefig('output.png', dpi=300)          # 기본 dpi=100

# 여백 최소화
plt.savefig('output.png', bbox_inches='tight')

# 배경색 + 여백 설정
plt.savefig('output.png',
            dpi=300,
            bbox_inches='tight',
            pad_inches=0.1,        # bbox_inches='tight'일 때 여백
            facecolor='#eeeeee',   # 배경색
            edgecolor='none')      # 테두리 색
```

### 지원 포맷

`png`, `pdf`, `svg`, `eps`, `ps`, `jpg`
