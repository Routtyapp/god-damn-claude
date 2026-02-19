Matplotlib은 데이터 시각화와 2D 그래프 플롯에 사용되는 파이썬 라이브러리입니다.

Matplotlib을 이용하면 아래 그림과 같이 다양한 유형의 그래프를 간편하게 그릴 수 있습니다.

Matplotlib과 그 종속 항목 (Dependencies)은 Windows, macOS, Linux에 대해 wheel 패키지의 형태로 배포됩니다.

아래의 명령어로 공식 배포판을 설치하세요.

python -m pip install -U pip
python -m pip install -U matplotlib

Third-party 배포판 설치하기
과학용 파이썬 배포판
Matplotlib 라이브러리와 함께 여러 유용한 데이터 과학 패키지를 포함하는 Anaconda와 ActiveState도
Matplotlib을 사용하는 좋은 방법이 될 수 있습니다.


Linux: 패키지 매니저 사용하기
대부분의 주요한 Linux 배포판에 대해서 패키지 매니저를 통해 Matplotlib를 설치할 수 있습니다.

Debian / Ubuntu: sudo apt-get install python3-matplotlib
Fedora: sudo dnf install python3-matplotlib
Red Hat: sudo yum install python3-matplotlib
Arch: sudo pacman -S python-matplotlib

Source로부터 설치하기
Matplotlib는 소스 디렉토리부터 간단하게 설치할 수 있습니다.

python -m pip install .
설치 방법에 대한 더 자세한 설명은 링크를 참고하세요.

**Pyplot 소개**
matplotlib.pyplot 모듈은 MATLAB과 비슷하게 명령어 스타일로 동작하는 함수의 모음입니다.

matplotlib.pyplot 모듈의 각각의 함수를 사용해서 간편하게 그래프를 만들고 변화를 줄 수 있습니다.

예를 들어, 그래프 영역을 만들고, 몇 개의 선을 표현하고, 레이블로 꾸미는 등의 일을 할 수 있습니다.

 
 
 
 

**기본 그래프 그리기**
예제1

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4])
plt.show()
pyplot.plot() 함수에 하나의 숫자 리스트를 입력함으로써 아래와 같은 그래프가 그려집니다.

plot() 함수는 리스트의 값들이 y 값들이라고 가정하고, x 값 [0, 1, 2, 3]을 자동으로 만들어냅니다.

matplotlib.pyplot 모듈의 show() 함수는 그래프를 화면에 나타나도록 합니다.

예제2

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.show()
plot() 함수는 다양한 기능을 포함하고 있어서, 임의의 개수의 인자를 받을 수 있습니다.

예를 들어, 아래와 같이 입력하면, x-y 값을 그래프로 나타낼 수 있습니다.

**스타일 지정하기**

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
plt.axis([0, 6, 0, 20])
plt.show()
x, y 값 인자에 대해 선의 색상과 형태를 지정하는 포맷 문자열 (Format string)을 세번째 인자에 입력할 수 있습니다.

포맷 문자열 ‘ro’는 빨간색 (‘red’)의 원형 (‘o’) 마커를 의미합니다.

또한, 예를 들어 ‘b-‘는 파란색 (‘blue’)의 실선 (‘-‘)을 의미합니다.

matplotlib.pyplot 모듈의 axis() 함수를 이용해서 축의 범위 [xmin, xmax, ymin, ymax]를 지정했습니다.

결과는 아래와 같습니다.

**여러 개의 그래프 그리기**

예제

import matplotlib.pyplot as plt
import numpy as np

# 200ms 간격으로 균일하게 샘플된 시간
t = np.arange(0., 5., 0.2)

# 빨간 대쉬, 파란 사각형, 녹색 삼각형
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
plt.show()
Matplotlib에서는 일반적으로 NumPy 어레이를 이용하게 되는데,

사실 NumPy 어레이를 사용하지 않더라도 모든 시퀀스는 내부적으로 NumPy 어레이로 변환됩니다.

이 예제는 다양한 스타일을 갖는 여러 개의 곡선을 하나의 그래프로 나타냅니다.

02. Matplotlib 숫자 입력하기

matplotlib.pyplot 모듈의 plot()은 선 (Line) 또는 마커 (Marker) 그래프 그리기에 사용되는 함수입니다.

plot() 함수에 y 값을 리스트 형태로 입력하면 위 그림과 같은 꺾은선 그래프가 그려집니다.

Keyword: matplotlib.pyplot, plt.plot()

**기본 사용**

예제

import matplotlib.pyplot as plt

plt.plot([2, 3, 5, 10])
plt.show()
이 세 줄의 코드는 간단한 그래프를 하나 띄웁니다.

plot([2, 3, 5, 10])와 같이 하나의 리스트 형태로 값들을 입력하면 y 값으로 인식합니다.

plot((2, 3, 5, 10)) 또는 plot(np.array([2, 3, 5, 10])와 같이 파이썬 튜플 또는 Numpy 어레이의 형태로도

데이터를 입력할 수 있습니다.

x 값은 기본적으로 [0, 1, 2, 3]이 되어서, 점 (0, 2), (1, 3), (2, 5), (3, 10)를 잇는 아래와 같은 꺾은선 그래프가 나타납니다.

x, y 값 입력하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.show()
plot() 함수에 두 개의 리스트를 입력하면 순서대로 x, y 값들로 인식해서

점 (1, 2), (2, 3), (3, 5), (4, 10)를 잇는 꺾은선 그래프가 나타납니다.

결과는 아래와 같습니다.

**레이블이 있는 데이터 사용하기**

예제

import matplotlib.pyplot as plt

data_dict = {'data_x': [1, 2, 3, 4, 5], 'data_y': [2, 3, 5, 10, 8]}

plt.plot('data_x', 'data_y', data=data_dict)
plt.show()
파이썬 딕셔너리와 같이 레이블이 있는 데이터를 그래프로 나타낼 수 있습니다.

예제에서와 같이, 먼저 plot() 함수에 데이터의 레이블 (딕셔너리의 키)을 입력해주고,

data 파라미터에 딕셔너리를 지정해줍니다.

03. Matplotlib 축 레이블 설정하기

matplotlib.pyplot 모듈의 xlabel(), ylabel() 함수를 사용하면 그래프의 x, y 축에 대한 레이블을 표시할 수 있습니다.

이 페이지에서는 xlabel(), ylabel() 함수를 사용해서 그래프의 축에 레이블을 표시하는 방법에 대해 소개합니다.

Keyword: plt.xlabel(), plt.ylabel(), plt.axis(), labelpad, fontdict, loc

기본 사용

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.xlabel('X-Label')
plt.ylabel('Y-Label')
plt.show()
xlabel(), ylabel() 함수에 문자열을 입력하면, 아래 그림과 같이 각각의 축에 레이블이 표시됩니다.

**여백 지정하기**

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', labelpad=15)
plt.ylabel('Y-Axis', labelpad=20)
plt.show()
xlabel(), ylabel() 함수의 labelpad 파라미터는 축 레이블의 여백 (Padding)을 지정합니다.

예제에서는 X축 레이블에 대해서 15pt, Y축 레이블에 대해서 20pt 만큼의 여백을 지정했습니다.

결과는 아래와 같습니다.

**폰트 설정하기**

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', labelpad=15, fontdict={'family': 'serif', 'color': 'b', 'weight': 'bold', 'size': 14})
plt.ylabel('Y-Axis', labelpad=20, fontdict={'family': 'fantasy', 'color': 'deeppink', 'weight': 'normal', 'size': 'xx-large'})
plt.show()
xlabel(), ylabel() 함수의 fontdict 파라미터를 사용하면 축 레이블의 폰트 스타일을 설정할 수 있습니다.

예제에서는 ‘family’, ‘color’, ‘weight’, ‘size’와 같은 속성을 사용해서 축 레이블 텍스트를 설정했습니다.

아래와 같이 작성하면 폰트 스타일을 편리하게 재사용할 수 있습니다.

import matplotlib.pyplot as plt

font1 = {'family': 'serif',
         'color': 'b',
         'weight': 'bold',
         'size': 14
         }

font2 = {'family': 'fantasy',
         'color': 'deeppink',
         'weight': 'normal',
         'size': 'xx-large'
         }

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', labelpad=15, fontdict=font1)
plt.ylabel('Y-Axis', labelpad=20, fontdict=font2)
plt.show()

위치 지정하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis', loc='right')
plt.ylabel('Y-Axis', loc='top')
plt.show()
xlabel() 함수의 loc 파라미터는 X축 레이블의 위치를 지정합니다. ({‘left’, ‘center’, ‘right’})

ylabel() 함수의 loc 파라미터는 Y축 레이블의 위치를 지정합니다. ({‘bottom’, ‘center’, ‘top’})

이 파라미터는 Matplotlib 3.3 이후 버전부터 적용되었습니다.

결과는 아래와 같습니다.

04. Matplotlib 범례 표시하기

범례 (Legend)는 그래프에 데이터의 종류를 표시하기 위한 텍스트입니다.

matplotlib.pyplot 모듈의 legend() 함수를 사용해서 그래프에 범례를 표시할 수 있습니다.

이 페이지에서는 그래프에 다양한 방식으로 범례를 표시하는 방법에 대해 소개합니다.

Keyworkd: plt.legend(), label, loc, ncol, fontsize, frameon, shadow, 범례

기본 사용

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.legend()

plt.show()

그래프 영역에 범례를 나타내기 위해서는 우선 plot() 함수에 label 문자열을 지정하고,
matplotlib.pyplot 모듈의 legend() 함수를 호출합니다.

아래와 같이 그래프의 적절한 위치에 데이터를 설명하는 범례가 나타납니다.

위치 지정하기

예제1

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc=(0.0, 0.0))
# plt.legend(loc=(0.5, 0.5))
plt.legend(loc=(1.0, 1.0))

plt.show()
xlabel(), ylabel() 함수의 labelpad 파라미터는 축 레이블의 여백 (Padding)을 지정합니다.

legend() 함수의 loc 파라미터를 이용해서 범례가 표시될 위치를 설정할 수 있습니다.

loc 파라미터를 숫자 쌍 튜플로 지정하면, 해당하는 위치에 범례가 표시됩니다.

loc=(0.0, 0.0)은 데이터 영역의 왼쪽 아래, loc=(1.0, 1.0)은 데이터 영역의 오른쪽 위 위치입니다.

loc 파라미터에 여러 숫자 쌍을 입력하면서 범례의 위치를 확인해보세요.

예제2

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.legend(loc='lower right')

plt.show()
loc 파라미터는 예제에서와 같이 문자열로 지정할 수도 있고, 숫자 코드를 사용할 수도 있습니다.

loc=’lower right’와 같이 지정하면 아래와 같이 오른쪽 아래에 범례가 표시됩니다.

열 개수 지정하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.plot([1, 2, 3, 4], [3, 5, 9, 7], label='Demand (#)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc='best')          # ncol = 1
plt.legend(loc='best', ncol=2)    # ncol = 2

plt.show()
legend() 함수의 ncol 파라미터는 범례에 표시될 텍스트의 열의 개수를 지정합니다.

기본적으로 아래 첫번째 그림과 같이 범례 텍스트는 1개의 열로 표시되며,

ncol=2로 지정하면 아래 두번째 그림과 같이 표시됩니다.

폰트 크기 지정하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.plot([1, 2, 3, 4], [3, 5, 9, 7], label='Demand (#)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc='best')
plt.legend(loc='best', ncol=2, fontsize=14)

plt.show()
legend() 함수의 fontsize 파라미터는 범례에 표시될 폰트의 크기를 지정합니다.

범례 테두리 꾸미기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], label='Price ($)')
plt.plot([1, 2, 3, 4], [3, 5, 9, 7], label='Demand (#)')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.legend(loc='best')
plt.legend(loc='best', ncol=2, fontsize=14, frameon=True, shadow=True)

plt.show()
frameon 파라미터는 범례 텍스트 상자의 테두리를 표시할지 여부를 지정합니다.
frameon=False로 지정하면 테두리가 표시되지 않습니다.

shadow 파라미터를 사용해서 텍스트 상자에 그림자를 표시할 수 있습니다.

결과는 아래와 같습니다.

이 외에도 legend() 함수에는 facecolor, edgecolor, borderpad, labelspacing과 같은 다양한 파라미터가 있습니다.

더 자세한 내용은 공식 홈페이지의 설명 (링크)을 참고하세요.

05. Matplotlib 축 범위 지정하기

matplotlib.pyplot 모듈의 xlim(), ylim(), axis() 함수를 사용하면 그래프의 X, Y축이 표시되는 범위를 지정할 수 있습니다.

xlim() - X축이 표시되는 범위를 지정하거나 반환합니다.
ylim() - Y축이 표시되는 범위를 지정하거나 반환합니다.
axis() - X, Y축이 표시되는 범위를 지정하거나 반환합니다.
이 페이지에서는 그래프의 축의 범위를 지정하고, 확인하는 방법에 대해 소개합니다.

Keyword: plt.xlim(), plt.ylim(), plt.axis(), 축 범위

기본 사용 - xlim(), ylim()

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.xlim([0, 5])      # X축의 범위: [xmin, xmax]
plt.ylim([0, 20])     # Y축의 범위: [ymin, ymax]

plt.show()
xlim() 함수에 xmin, xmax 값을 각각 입력하거나 리스트 또는 튜플의 형태로 입력합니다.

ylim() 함수에 ymin, ymax 값을 각각 입력하거나 리스트 또는 튜플의 형태로 입력합니다.

입력값이 없으면 데이터에 맞게 자동으로 범위를 지정합니다.

결과는 아래와 같습니다.

기본사용 - axis()

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0, 5, 0, 20])  # X, Y축의 범위: [xmin, xmax, ymin, ymax]

plt.show()
axis() 함수에 [xmin, xmax, ymin, ymax]의 형태로 X, Y축의 범위를 지정할 수 있습니다.

axis() 함수에 입력한 리스트 (또는 튜플)는 반드시 네 개의 값 (xmin, xmax, ymin, ymax)이 있어야 합니다.

입력값이 없으면 데이터에 맞게 자동으로 범위를 지정합니다.

옵션 지정하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
# plt.axis('square')
plt.axis('scaled')

plt.show()
axis() 함수는 아래와 같이 축에 관한 다양한 옵션을 제공합니다.

'on' | 'off' | 'equal' | 'scaled' | 'tight' | 'auto' | 'normal' | 'image' | 'square'
 

아래의 그림은 ‘square’로 지정했을 때의 그래프입니다.

축의 길이가 동일하게 표시됩니다.

아래의 그림은 ‘scaled’로 지정했을 때의 그래프입니다.

X, Y축이 같은 길이 스케일로 나타나게 됩니다.

축 범위 얻기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10])
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')

x_range, y_range = plt.xlim(), plt.ylim()
print(x_range, y_range)

axis_range = plt.axis('scaled')
print(axis_range)

plt.show()
xlim(), ylim() 함수는 그래프 영역에 표시되는 X축, Y축의 범위를 각각 반환합니다.

axis() 함수는 그래프 영역에 표시되는 X, Y축의 범위를 반환합니다.

(0.85, 4.15) (1.6, 10.4)
(0.85, 4.15, 1.6, 10.4)
위의 예제 그림에서 X축은 0.85에서 4.15, Y축은 1.6에서 10.4 범위로 표시되었음을 알 수 있습니다.  

06. Matplotlib 선 종류 지정하기

데이터를 표현하기 위해 그려지는 선의 종류를 지정하는 방법을 소개합니다.

선 종류를 나타내는 문자열 또는 튜플을 이용해서 다양한 선의 종류를 구현할 수 있습니다.

Keyword: plt.plot(), 포맷 문자열, 선 종류, linestyle, Solid, Dashed, Dotted, Dash-dot

기본 사용

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3], [4, 4, 4], '-', color='C0', label='Solid')
plt.plot([1, 2, 3], [3, 3, 3], '--', color='C0', label='Dashed')
plt.plot([1, 2, 3], [2, 2, 2], ':', color='C0', label='Dotted')
plt.plot([1, 2, 3], [1, 1, 1], '-.', color='C0', label='Dash-dot')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=4)
plt.show()
Matplotlib에서 선의 종류를 지정하는 가장 간단한 방법은 포맷 문자열을 사용하는 것입니다.

‘ - ‘ (Solid), ‘ - - ‘ (Dashed), ‘ : ‘ (Dotted), ‘ -. ‘ (Dash-dot)과 같이 네가지 종류를 선택할 수 있습니다.

아래와 같은 선이 표시됩니다.

linestyle 지정하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3], [4, 4, 4], linestyle='solid', color='C0', label="'solid'")
plt.plot([1, 2, 3], [3, 3, 3], linestyle='dashed', color='C0', label="'dashed'")
plt.plot([1, 2, 3], [2, 2, 2], linestyle='dotted', color='C0', label="'dotted'")
plt.plot([1, 2, 3], [1, 1, 1], linestyle='dashdot', color='C0', label="'dashdot'")
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=4)
plt.tight_layout()
plt.show()
plot() 함수의 linestyle 파라미터 값을 직접 지정할 수 있습니다.

포맷 문자열과 같이 ‘solid’, ‘dashed’, ‘dotted’, dashdot’ 네가지의 선 종류를 지정할 수 있습니다.

결과는 아래와 같습니다.

튜플 사용하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3], [4, 4, 4], linestyle=(0, (1, 1)), color='C0', label='(0, (1, 1))')
plt.plot([1, 2, 3], [3, 3, 3], linestyle=(0, (1, 5)), color='C0', label='(0, (1, 5))')
plt.plot([1, 2, 3], [2, 2, 2], linestyle=(0, (5, 1)), color='C0', label='(0, (5, 1))')
plt.plot([1, 2, 3], [1, 1, 1], linestyle=(0, (3, 5, 1, 5)), color='C0', label='(0, (3, 5, 1, 5))')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=2)
plt.tight_layout()
plt.show()
튜플을 사용해서 선의 종류를 커스터마이즈할 수 있습니다.

예를 들어, (0, (1, 1))은 ‘dotted’와 같고, (0, (5, 5))는 ‘dashed’와 같습니다.

또한 (0, (3, 5, 1, 5))는 ‘dashdotted’와 같습니다.

숫자를 바꿔가면서 다양한 선의 종류를 만들어보세요.

결과는 아래와 같습니다.

선 끝 모양 지정하기
 

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3], [4, 4, 4], linestyle='solid', linewidth=10,
      solid_capstyle='butt', color='C0', label='solid+butt')
plt.plot([1, 2, 3], [3, 3, 3], linestyle='solid', linewidth=10,
      solid_capstyle='round', color='C0', label='solid+round')

plt.plot([1, 2, 3], [2, 2, 2], linestyle='dashed', linewidth=10,
      dash_capstyle='butt', color='C1', label='dashed+butt')
plt.plot([1, 2, 3], [1, 1, 1], linestyle='dashed', linewidth=10,
      dash_capstyle='round', color='C1', label='dashed+round')


plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.axis([0.8, 3.2, 0.5, 5.0])
plt.legend(loc='upper right', ncol=2)
plt.tight_layout()
plt.show()
plot() 함수의 solid_capstyle, dash_capstyle를 사용해서 선의 끝 모양을 지정할 수 있습니다.

각각 ‘butt’, ‘round’로 지정하면 아래 그림과 같이 뭉뚝한, 둥근 끝 모양이 나타납니다.

더 다양한 선 종류는 아래 그림을 참고하세요.

Named Linestyles
1. solid
2. dotted
3. dashed
4. dashdot
parametrized linestyles
1. loosely dotted (0, (1, 10))
2. dotted (0, (1, 1))
3. densely dotted (0, (1, 1))
4. loosely dashed (0, 5, 10))
5. dashed (0, (5, 5))
6. densely dashed (0, (5,1))
7. loosely dashdotted (0, (3, 10, 1, 10))
8. dashdotted (0, (3, 1, 1, 1))
9. densely dashdotted (0, (3, 5, 1, 5, 1, 5))
10. dashdotted (0, (3, 5, 1, 5, 1, 5))
11. loosely dashdotdotted (0, (3, 10 ,1, 10, 1, 10))
12. densely dashdotdotted (0, (3, 1, 1, 1, 1, 1))

# Matplotlib 마커 지정하기 #
특별한 설정이 없으면 그래프가 실선으로 그려지지만, 위의 그림과 같은 마커 형태의 그래프를 그릴 수 있습니다.

plot() 함수의 포맷 문자열 (Format string)을 사용해서 그래프의 선과 마커를 지정하는 방법에 대해 알아봅니다.

Keyword: plt.plot(), 포맷 문자열, 마커, marker

기본 사용
예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], 'bo')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.show()
plot() 함수에 ‘bo’를 입력해주면 파란색의 원형 마커로 그래프가 표시됩니다.

‘b’는 blue, ‘o’는 circle을 나타내는 문자입니다.

결과는 아래와 같습니다.

선/마커 동시에 나타내기

예제

import matplotlib.pyplot as plt

# plt.plot([1, 2, 3, 4], [2, 3, 5, 10], 'bo-')    # 파란색 + 마커 + 실선
plt.plot([1, 2, 3, 4], [2, 3, 5, 10], 'bo--')     # 파란색 + 마커 + 점선
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.show()
‘bo-‘는 파란색의 원형 마커와 실선 (Solid line)을 의미합니다.

또한 ‘bo- -‘는 파란색의 원형 마커와 점선 (Dashed line)을 의미합니다.

아래 그림과 같이 실선 또는 점선과 마커가 함께 표시됩니다.

선/마커 표시 형식
선/마커 표시 형식에 대한 예시는 아래와 같습니다.

'b'    # blue markers with default shape
'ro'   # red circles
'g-'   # green solid line
'--'   # dashed line with default color
'k^:'  # black triangle_up markers connected by a dotted line

Colors
character	color
'b'	blue
'g'	green
'r'	red
'c'	cyan
'm'	magenta
'y'	yellow
'k'	black
'w'	white
Line Styles
character	description
'-'	solid line style
'--'	dashed line style
'-.'	dash-dot line style
':'	dotted line style
Markers
character	description
'.'	point marker
','	pixel marker
'o'	circle marker
'v'	triangle_down marker
'^'	triangle_up marker
'<'	triangle_left marker
'>'	triangle_right marker
'1'	tri_down marker
'2'	tri_up marker
'3'	tri_left marker
'4'	tri_right marker
's'	square marker
'p'	pentagon marker
'*'	star marker
'h'	hexagon1 marker
'H'	hexagon2 marker
'+'	plus marker
'x'	x marker
'D'	diamond marker
'd'	thin_diamond marker
`'	'`
'_'	hline marker


marker 파라미터 사용하기

예제

import matplotlib.pyplot as plt

plt.plot([4, 5, 6], marker="H")
plt.plot([3, 4, 5], marker="d")
plt.plot([2, 3, 4], marker="x")
plt.plot([1, 2, 3], marker=11)
plt.plot([0, 1, 2], marker='$Z$')
plt.show()
plot() 함수의 marker 파라미터를 사용하면 더욱 다양한 마커 형태를 지정할 수 있습니다.

예제에서 다섯가지 마커를 지정했습니다.

Matplotlib에서 사용할 수 있는 모든 마커의 종류를 아래 표에서 참고하세요.

Marker Table
marker	description
"."	point
","	pixel
"o"	circle
"v"	triangle_down
"^"	triangle_up
"<"	triangle_left
">"	triangle_right
"1"	tri_down
"2"	tri_up
"3"	tri_left
"4"	tri_right
"8"	octagon
"s"	square
"p"	pentagon
"P"	plus (filled)
"*"	star
"h"	hexagon1
"H"	hexagon2
"+"	plus
"x"	x
"X"	x (filled)
"D"	diamond
"d"	thin_diamond
`"	"`
"_"	hline
0 (TICKLEFT)	tickleft
1 (TICKRIGHT)	tickright
2 (TICKUP)	tickup
3 (TICKDOWN)	tickdown
4 (CARETLEFT)	caretleft
5 (CARETRIGHT)	caretright
6 (CARETUP)	caretup
7 (CARETDOWN)	caretdown
8 (CARETLEFTBASE)	caretleft (centered at base)
9 (CARETRIGHTBASE)	caretright (centered at base)
10 (CARETUPBASE)	caretup (centered at base)
11 (CARETDOWNBASE)	caretdown (centered at base)
"None", " " or ""	nothing
"$...$"	Render the string using mathtext (e.g. "$f$" for marker showing the letter f)

# Matplotlib 색상 지정하기 #
matplotlib.pyplot 모듈의 plot() 함수를 사용해서 그래프를 나타낼 때, 색상을 지정하는 다양한 방법에 대해 소개합니다.

Keyword: plt.plot(), 포맷 문자열, 그래프 색상, color, Hex code, Matplotlib 색상

포맷 문자열 사용하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2.0, 3.0, 5.0, 10.0], 'r')
plt.plot([1, 2, 3, 4], [2.0, 2.8, 4.3, 6.5], 'g')
plt.plot([1, 2, 3, 4], [2.0, 2.5, 3.3, 4.5], 'b')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')

plt.show()
plot() 함수의 포맷 문자열 (Format string)을 사용해서 실선의 색상을 지정했습니다.

결과는 아래와 같습니다.

color 키워드 인자 사용하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2.0, 3.0, 5.0, 10.0], color='limegreen')
plt.plot([1, 2, 3, 4], [2.0, 2.8, 4.3, 6.5], color='violet')
plt.plot([1, 2, 3, 4], [2.0, 2.5, 3.3, 4.5], color='dodgerblue')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')

plt.show()
color 키워드 인자를 사용해서 더 다양한 색상의 이름을 지정할 수 있습니다.

plot() 함수에 color=’limegreen’과 같이 입력하면, limegreen에 해당하는 색깔이 표시됩니다.

결과는 아래와 같습니다.

Hex code 사용하기

예제

import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [2, 3, 5, 10], color='#e35f62',
         marker='o', linestyle='--')
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')

plt.show()
16진수 코드 (Hex code)로 더욱 다양한 색상을 지정할 수 있습니다.

이번에는 선의 색상과 함께 마커와 선의 종류까지 모두 지정해 보겠습니다.

marker는 마커 스타일, linestyle는 선의 스타일을 지정합니다.

선의 색상은 Hex code ‘#e35f62’로, 마커는 원형 (Circle), 선 종류는 대시 (Dashed)로 지정했습니다.

Matplotlib 색상들
Matplotlib에서 사용할 수 있는 다양한 색상의 이름을 아래에서 확인하세요.

Cycler 색상
색상을 지정하지 않으면 기본적으로 아래의 10개의 색상이 반복적으로 표시됩니다.

기본 색상
아래의 색상은 간단한 문자열을 사용해서 색상을 지정하도록 합니다.

Tableau 색상
Matplotlib은 Tableau의 색상들을 지원하며, 이는 Cycler의 색상과 같습니다.

CSS 색상
아래와 같이 색상의 이름을 사용해서 색상을 지정할 수 있습니다.

Color Names
Grayscale / Neutral

black
dimgray, dimgrey
gray, grey
darkgray, darkgrey
silver
lightgray, lightgrey
gainsboro
whitesmoke
white
snow
Reds / Browns / Oranges
rosybrown
lightcoral
indianred
brown
firebrick
maroon
darkred
red
mistyrose
salmon
tomato
darksalmon
coral
orangered
lightsalmon
sienna
seashell
chocolate
saddlebrown
sandybrown
peachpuff
peru
linen
Yellows / Golds / Beiges
bisque
darkorange
burlywood
antiquewhite
tan
navajowhite
blanchedalmond
papayawhip
moccasin
orange
wheat
oldlace
floralwhite
darkgoldenrod
goldenrod
cornsilk
gold
lemonchiffon
khaki
palegoldenrod
darkkhaki
ivory
beige
lightyellow
lightgoldenrodyellow
Greens
olive
yellow
olivedrab
yellowgreen
darkolivegreen
greenyellow
chartreuse
lawngreen
honeydew
darkseagreen
palegreen
lightgreen
forestgreen
limegreen
darkgreen
green
lime
seagreen
mediumseagreen
springgreen
mintcream
mediumspringgreen
Cyans / Teals
mediumaquamarine
aquamarine
turquoise
lightseagreen
mediumturquoise
azure
lightcyan
paleturquoise
darkslategray, darkslategrey
teal
darkcyan
aqua
cyan
darkturquoise
Blues
cadetblue
powderblue
lightblue
deepskyblue
skyblue
lightskyblue
steelblue
aliceblue
dodgerblue
lightslategray, lightslategrey
slategray, slategrey
slateblue
darkslateblue
mediumslateblue
cornflowerblue
royalblue
ghostwhite
lavender
midnightblue
navy
darkblue
mediumblue
blue
Purples / Pinks
mediumpurple
rebeccapurple
blueviolet
indigo
darkorchid
darkviolet
mediumorchid
thistle
plum
violet
purple
darkmagenta
fuchsia
magenta
orchid
mediumvioletred
deeppink
hotpink
lavenderblush
palevioletred
crimson
pink
lightpink

# Matplotlib 그래프 영역 채우기 #
그래프의 특정 영역을 색상으로 채워서 강조할 수 있습니다.

matplotlib.pyplot 모듈에서 그래프의 영역을 채우는 아래의 세가지 함수에 대해 소개합니다.

fill_between() - 두 수평 방향의 곡선 사이를 채웁니다.
fill_betweenx() - 두 수직 방향의 곡선 사이를 채웁니다.
fill() - 다각형 영역을 채웁니다.
Keyword: plt.fill_between(), plt.fill_betweenx(), plt.fill()

기본 사용 - fill_between()

예제

import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [2, 3, 5, 10]

plt.plot(x, y)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill_between(x[1:3], y[1:3], alpha=0.5)

plt.show()
fill_between() 함수에 x[1:3], y[1:3]를 순서대로 입력하면,

네 점 (x[1], y[1]), (x[2], y[2]), (x[1], 0), (x[2], 0)을 잇는 영역이 채워집니다.

기본 사용 - fill_betweenx()

예제

import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [2, 3, 5, 10]

plt.plot(x, y)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill_betweenx(y[2:4], x[2:4], alpha=0.5)

plt.show()
fill_betweenx() 함수에 y[2:4], x[2:4]를 순서대로 입력하면,

네 점 (x[2], y[2]), (x[3], y[3]), (0, y[2]), (0, y[3])을 잇는 영역이 채워집니다.

두 그래프 사이 영역 채우기

예제

import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y1 = [2, 3, 5, 10]
y2 = [1, 2, 4, 8]

plt.plot(x, y1)
plt.plot(x, y2)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill_between(x[1:3], y1[1:3], y2[1:3], color='lightgray', alpha=0.5)

plt.show()
두 개의 그래프 사이 영역을 채우기 위해서 두 개의 y 값의 리스트 y1, y2를 입력해줍니다.

네 점 (x[1], y[1]), (x[1], y[2]), (x[2], y[1]), (x[2], y[2]) 사이 영역을 채웁니다.

결과는 아래와 같습니다.

다각형 영역 채우기 - fill()

예제

import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y1 = [2, 3, 5, 10]
y2 = [1, 2, 4, 8]

plt.plot(x, y1)
plt.plot(x, y2)
plt.xlabel('X-Axis')
plt.ylabel('Y-Axis')
plt.fill([1.9, 1.9, 3.1, 3.1], [1.0, 4.0, 6.0, 3.0], color='lightgray', alpha=0.5)

plt.show()
fill() 함수에 x, y 값의 리스트를 입력해주면,

각 x, y 점들로 정의되는 다각형 영역을 자유롭게 지정해서 채울 수 있습니다.

결과는 아래와 같습니다.

# Matplotlib 축 스케일 지정하기 #

matplotlib.pyplot 모듈의 xscale(), yscale() 함수를 사용해서 그래프의 축 스케일을 다양하게 지정할 수 있습니다.

축은 기본적으로 ‘linear’ 스케일로 표시되지만 ‘log’, ‘symlog’, ‘logit’으로 변경할 수 있습니다.

Keyword: plt.xscale(), plt.yscale(), axis scale, linear, log

X축 스케일 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-10, 10, 100)
y = x ** 3

plt.plot(x, y)
plt.xscale('symlog')

plt.show()
xscale() 함수를 사용해서 X축의 스케일을 ‘symlog’로 지정했습니다.

‘symlog’는 Symmetrical Log Scale입니다.

축의 원점을 기준으로 양, 음의 방향이 대칭적인 로그 스케일로 표시됩니다.

결과는 아래와 같습니다.

Y축 스케일 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 5, 100)
y = np.exp(x)

plt.plot(x, y)
# plt.yscale('linear')
plt.yscale('log')

plt.show()
yscale() 함수를 사용해서 Y축의 스케일을 지정했습니다.

로그 스케일은 지수함수 (Exponential function)와 같이 기하급수적으로 변화하는 데이터를 표현하기에 적합합니다.

아래의 그래프는 Y축을 선형 스케일 (Linear scale)로 나타낸 결과이고,

아래의 그래프는 Y축을 로그 스케일 (Log scale)로 나타낸 결과입니다.  

# Matplotlib 여러 곡선 그리기 #

하나의 그래프 영역에 여러 개의 곡선을 나타내는 방법에 대해 소개합니다.

Keyword: plt.plot(), 여러 곡선 그리기

기본 사용
예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'r--', x, x**2, 'bo', x, x**3, 'g-.')
plt.show()
NumPy 어레이 [ 0. 0.2 0.4 0.6 0.8 1. 1.2 1.4 1.6 1.8]를 만들었습니다.

plot() 함수에 x 값, y 값, 스타일을 순서대로 세 번씩 입력하면, 세 개의 곡선 (y=x, y=x^2, y=x^3)이 동시에 그려집니다.

‘r–‘은 빨간색 (Red)의 대쉬 (Dashed) 스타일 선,

‘bo’는 파란색 (Blue)의 원형 (Circle) 마커,

‘g-.’은 녹색 (Green)의 대쉬-닷 (Dash-dot) 스타일 선을 의미합니다.

결과는 아래와 같습니다.

스타일 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='forestgreen', marker='^', markersize=9)
plt.show()
plot() 함수를 여러 번 호출함으로써 각각의 곡선을 그래프에 표시하고 스타일을 설정할 수 있습니다.

첫 번째 곡선의 스타일은 ‘bo’, 두 번째 곡선은 color=’#e35f62’, marker=’*’, linewidth=2로,

세 번째 곡선은 color=’forestgreen’, marker=’^’, markersize=9로 각각 설정했습니다.

결과는 아래와 같습니다.

# Matplotlib 그리드 설정하기 #
데이터의 위치를 더 명확하게 나타내기 위해 그래프에 그리드 (Grid, 격자)를 표시할 수 있습니다.

이 페이지에서는 matplotlib.pyplot 모듈의 grid() 함수를 이용해서 그래프에 다양하게 그리드를 설정해 보겠습니다.

Keyword: plt.grid(), grid style, 그리드, 격자

기본 사용

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='springgreen', marker='^', markersize=9)
plt.grid(True)

plt.show()
plt.grid(True)와 같이 설정하면, 그래프의 x, y축에 대해 그리드가 표시됩니다.

축 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='forestgreen', marker='^', markersize=9)
plt.grid(True, axis='y')

plt.show()
axis=y로 설정하면 가로 방향의 그리드만 표시됩니다.

{‘both’, ‘x’, ‘y’} 중 선택할 수 있고 디폴트는 ‘both’입니다.

결과는 아래와 같습니다.

스타일 설정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='springgreen', marker='^', markersize=9)
plt.grid(True, axis='y', color='red', alpha=0.5, linestyle='--')

plt.show()
color, alpha, linestyle 파마리터를 사용해서 그리드 선의 스타일을 설정했습니다.

또한 which 파라미터를 ‘major’, ‘minor’, ‘both’ 등으로 사용하면 주눈금, 보조눈금에 각각 그리드를 표시할 수 있습니다.

결과는 아래와 같습니다.

# Matplotlib 눈금 표시하기 #
틱 (Tick)은 그래프의 축에 간격을 구분하기 위해 표시하는 눈금입니다.

matplotlib.pyplot 모듈의 xticks(), yticks(), tick_params() 함수를 이용해서

그래프에 눈금을 표시하는 방법에 대해 소개합니다.

Keyword: plt.xticks(), plt.yticks(), plt.tick_params(), tick, 눈금 스타일

기본 사용

import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='forestgreen', marker='^', markersize=9)
plt.xticks([0, 1, 2])
plt.yticks(np.arange(1, 6))

plt.show()
xticks(), yticks() 함수는 각각 X축, Y축에 눈금을 표시합니다.

xticks(), yticks() 함수에 파이썬 리스트 또는 NumPy 어레이를 입력하면 눈금과 숫자 레이블이 표시됩니다.

예제에서는 X축의 [0, 1, 2]의 위치, Y축의 [1, 2, 3, 4, 5]의 위치에 눈금을 표시했습니다.

결과는 아래와 같습니다.

눈금 레이블 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='springgreen', marker='^', markersize=9)
plt.xticks(np.arange(0, 2, 0.2), labels=['Jan', '', 'Feb', '', 'Mar', '', 'May', '', 'June', '', 'July'])
plt.yticks(np.arange(0, 7), ('0', '1GB', '2GB', '3GB', '4GB', '5GB', '6GB'))

plt.show()
labels 파라미터를 사용하면 눈금 레이블을 문자열의 형태로 지정할 수 있습니다.

입력해준 눈금의 개수와 같은 개수의 레이블을 지정해줍니다.

눈금 스타일 설정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='springgreen', marker='^', markersize=9)
plt.xticks(np.arange(0, 2, 0.2), labels=['Jan', '', 'Feb', '', 'Mar', '', 'May', '', 'June', '', 'July'])
plt.yticks(np.arange(0, 7), ('0', '1GB', '2GB', '3GB', '4GB', '5GB', '6GB'))

plt.tick_params(axis='x', direction='in', length=3, pad=6, labelsize=14, labelcolor='green', top=True)
plt.tick_params(axis='y', direction='inout', length=10, pad=15, labelsize=12, width=2, color='r')

plt.show()
tick_params() 함수를 사용하면 눈금의 스타일을 다양하게 설정할 수 있습니다.

axis는 설정이 적용될 축을 지정합니다. {‘x’, ‘y’, ‘both’} 중 선택할 수 있습니다.

direction을 ‘in’, ‘out’으로 설정하면 눈금이 안/밖으로 표시됩니다. {‘in’, ‘out’, ‘inout’} 중 선택할 수 있습니다.

length는 눈금의 길이를 지정합니다.

pad는 눈금과 레이블과의 거리를 지정합니다.

labelsize는 레이블의 크기를 지정합니다.

labelcolor는 레이블의 색상을 지정합니다.

top/bottom/left/right를 True/False로 지정하면 눈금이 표시될 위치를 선택할 수 있습니다.

width는 눈금의 너비를 지정합니다.

color는 눈금의 색상을 지정합니다.

결과는 아래와 같습니다.

# Matplotlib 타이틀 설정하기 #

matplotlib.pyplot 모듈의 title() 함수를 이용해서 그래프의 타이틀 (Title)을 설정할 수 있습니다.

이 페이지에서는 그래프의 타이틀을 표시하고 위치를 조절하는 방법, 그리고 타이틀의 폰트와 스타일을 설정하는 방법에 대해 알아봅니다.

Keyword: plt.title(), loc, pad, 그래프 제목, 스타일, 위치, 여백

기본 사용

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='forestgreen', marker='^', markersize=9)

plt.tick_params(axis='both', direction='in', length=3, pad=6, labelsize=14)
plt.title('Graph Title')

plt.show()
title() 함수를 이용해서 그래프의 타이틀을 ‘Graph Title’로 설정했습니다.

결과는 아래와 같습니다.

위치와 오프셋 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='forestgreen', marker='^', markersize=9)

plt.tick_params(axis='both', direction='in', length=3, pad=6, labelsize=14)
plt.title('Graph Title', loc='right', pad=20)

plt.show()
plt.title() 함수의 loc 파라미터를 ‘right’로 설정하면, 타이틀이 그래프의 오른쪽 위에 나타나게 됩니다.

{‘left’, ‘center’, ‘right’} 중 선택할 수 있으며 디폴트는 ‘center’입니다.

pad 파라미터는 타이틀과 그래프와의 간격을 포인트 단위로 설정합니다.

결과는 아래와 같습니다.

폰트 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='forestgreen', marker='^', markersize=9)

plt.tick_params(axis='both', direction='in', length=3, pad=6, labelsize=14)
plt.title('Graph Title', loc='right', pad=20)

title_font = {
    'fontsize': 16,
    'fontweight': 'bold'
}
plt.title('Graph Title', fontdict=title_font, loc='left', pad=20)

plt.show()
fontdict 파라미터에 딕셔너리 형태로 폰트 스타일을 설정할 수 있습니다.

‘fontsize’를 16으로, ‘fontweight’를 ‘bold’로 설정했습니다.

‘fontsize’는 포인트 단위의 숫자를 입력하거나 ‘smaller’, ‘x-large’ 등의 상대적인 설정을 할 수 있습니다.

‘fontweight’에는 {‘normal’, ‘bold’, ‘heavy’, ‘light’, ‘ultrabold’, ‘ultralight’}와 같이 설정할 수 있습니다.

결과는 아래와 같습니다.

타이틀 얻기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 2, 0.2)

plt.plot(x, x, 'bo')
plt.plot(x, x**2, color='#e35f62', marker='*', linewidth=2)
plt.plot(x, x**3, color='forestgreen', marker='^', markersize=9)

plt.tick_params(axis='both', direction='in', length=3, pad=6, labelsize=14)
title_right = plt.title('Graph Title', loc='right', pad=20)

title_font = {
    'fontsize': 16,
    'fontweight': 'bold'
}
title_left = plt.title('Graph Title', fontdict=title_font, loc='left', pad=20)

print(title_left.get_position())
print(title_left.get_text())

print(title_right.get_position())
print(title_right.get_text())

plt.show()
plt.title() 함수는 타이틀을 나타내는 Matplotlib text 객체를 반환합니다.

get_position()과 get_text() 메서드를 사용해서 텍스트 위치와 문자열을 얻을 수 있습니다.

(0.0, 1.0)
Graph Title
(1.0, 1.0)
Graph Title

# Matplotlib 수평선/수직선 표시하기 #

그래프의 특정 위치에 수직선/수평선을 표시하기 위해서 matplotlib.pyplot 모듈은 아래의 네가지 함수를 지원합니다.

axhline(): 축을 따라 수평선을 표시합니다.
axvline(): 축을 따라 수직선을 표시합니다.
hlines(): 지정한 점을 따라 수평선을 표시합니다.
vlines(): 지정한 점을 따라 수직선을 표시합니다.
그래프에 수직선/수평선을 표시하고, 스타일을 설정하는 방법을 소개합니다.

Keyword: plt.axhline(), plt.axvline(), plt.hlines(), plt.vlines(), 수직선, 수평선

수평선 그리기 - axhline(), hlines()

예제

import matplotlib.pyplot as plt

x = np.arange(0, 4, 0.5)

plt.plot(x, x + 1, 'bo')
plt.plot(x, x**2 - 4, 'g--')
plt.plot(x, -2*x + 3, 'r:')

plt.axhline(4.0, 0.1, 0.9, color='lightgray', linestyle='--', linewidth=2)
plt.hlines(-0.62, 1.0, 2.5, color='gray', linestyle='solid', linewidth=3)

plt.show()
axhline() 함수의 첫번째 인자는 y 값으로서 수평선의 위치가 됩니다.

두, 세번째 인자는 xmin, xmax 값으로서 0에서 1 사이의 값을 입력합니다. 0은 왼쪽 끝, 1은 오른쪽 끝을 의미합니다.

hlines() 함수에 y, xmin, xmax를 순서대로 입력하면, 점 (xmin, y)에서 점 (xmax, y)를 따라 수평선을 표시합니다.

결과는 아래와 같습니다.

수직선 그리기 - axvline(), vlines()

예제

import matplotlib.pyplot as plt

x = np.arange(0, 4, 0.5)

plt.plot(x, x + 1, 'bo')
plt.plot(x, x**2 - 4, 'g--')
plt.plot(x, -2*x + 3, 'r:')

plt.axvline(1.0, 0.2, 0.8, color='lightgray', linestyle='--', linewidth=2)
plt.vlines(1.8, -3.0, 2.0, color='gray', linestyle='solid', linewidth=3)

plt.show()
axvline() 함수의 첫번째 인자는 x 값으로서 수직선의 위치가 됩니다.

두, 세번째 인자는 ymin, ymax 값으로서 0에서 1 사이의 값을 입력합니다. 0은 아래쪽 끝, 1은 위쪽 끝을 의미합니다.

vlines() 함수에 x, ymin, ymax를 순서대로 입력하면, 점 (x, ymin)에서 점 (x, ymax)를 따라 수평선을 표시합니다.

# Matplotlib 막대 그래프 그리기 #

막대 그래프 (Bar graph, Bar chart)는 범주가 있는 데이터 값을 직사각형의 막대로 표현하는 그래프입니다.

Matplotlib에서는 matplotlib.pyplot 모듈의 bar() 함수를 이용해서 막대 그래프를 간단하게 표현할 수 있습니다.

Keyword: plt.bar(), bar graph, 막대 그래프

기본 사용

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.bar(x, values)
plt.xticks(x, years)

plt.show()
이 예제는 연도별로 변화하는 값을 갖는 데이터를 막대 그래프로 나타냅니다.

NumPy의 np.arange() 함수는 주어진 범위와 간격에 따라 균일한 값을 갖는 어레이를 반환합니다.

years는 X축에 표시될 연도이고, values는 막대 그래프의 y 값 입니다.

먼저 plt.bar() 함수에 x 값 [0, 1, 2]와 y 값 [100, 400, 900]를 입력해주고,

xticks()에 x와 years를 입력해주면, X축의 눈금 레이블에 ‘2018’, ‘2019’, ‘2020’이 순서대로 표시됩니다.

결과는 아래와 같습니다.

색상 지정하기

예제1

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.bar(x, values, color='y')
# plt.bar(x, values, color='dodgerblue')
# plt.bar(x, values, color='C2')
# plt.bar(x, values, color='#e35f62')
plt.xticks(x, years)

plt.show()
plt.bar() 함수의 color 파라미터를 사용해서 막대의 색상을 지정할 수 있습니다.

예제에서는 네 가지의 색상을 사용했습니다.

Matplotlib의 다양한 색상에 대해서는 Matplotlib 색상 지정하기 페이지를 참고하세요.

예제2

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]
colors = ['y', 'dodgerblue', 'C2']

plt.bar(x, values, color=colors)
plt.xticks(x, years)

plt.show()
plt.bar() 함수의 color 파라미터에 색상의 이름을 리스트의 형태로 입력하면,

막대의 색상을 각각 다르게 지정할 수 있습니다.

막대 폭 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.bar(x, values, width=0.4)
# plt.bar(x, values, width=0.6)
# plt.bar(x, values, width=0.8)
# plt.bar(x, values, width=1.0)
plt.xticks(x, years)

plt.show()
plt.bar() 함수의 width 파라미터는 막대의 폭을 지정합니다.

예제에서는 막대의 폭을 0.4/0.6/0.8/1.0으로 지정했고, 디폴트는 0.8입니다.

스타일 꾸미기

예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.bar(x, values, align='edge', edgecolor='lightgray',
        linewidth=5, tick_label=years)

plt.show()
이번에는 막대 그래프의 테두리의 색, 두께 등 스타일을 적용해 보겠습니다.

align은 눈금과 막대의 위치를 조절합니다. 디폴트 값은 ‘center’이며, ‘edge’로 설정하면 막대의 왼쪽 끝에 눈금이 표시됩니다.

edgecolor는 막대 테두리 색, linewidth는 테두리의 두께를 지정합니다.

tick_label을 리스트 또는 어레이 형태로 지정하면, 틱에 문자열을 순서대로 나타낼 수 있습니다.

# Matplotlib 수평 막대 그래프 그리기 #

수평 막대 그래프 (Horizontal bar graph)는 범주가 있는 데이터 값을 수평 막대로 표현하는 그래프입니다.

matplotlib.pyplot 모듈의 barh() 함수를 사용해서 수평 막대 그래프를 그리는 방법을 소개합니다.

Keyword: plt.barh(), horizontal bar graph, 수평 막대 그래프

기본 사용

예제

import matplotlib.pyplot as plt
import numpy as np

y = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.barh(y, values)
plt.yticks(y, years)

plt.show()
연도별로 변화하는 값을 갖는 데이터를 수평 막대 그래프로 나타냈습니다.

years는 Y축에 표시될 연도이고, values는 막대 그래프의 너비로 표시될 x 값 입니다.

먼저 barh() 함수에 NumPy 어레이 [0, 1, 2]와 x 값에 해당하는 리스트 [100, 400, 900]를 입력해줍니다.

다음, yticks()에 y와 years를 입력해주면, Y축의 눈금 레이블에 ‘2018’, ‘2019’, ‘2020’이 순서대로 표시됩니다.

결과는 아래와 같습니다.

색상 지정하기

예제1

import matplotlib.pyplot as plt
import numpy as np

y = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.barh(y, values, color='y')
# plt.barh(y, values, color='dodgerblue')
# plt.barh(y, values, color='C2')
# plt.barh(y, values, color='#e35f62')
plt.yticks(y, years)

plt.show()
plt.barh() 함수의 color 파라미터를 사용해서 막대의 색상을 지정할 수 있습니다.

예제에서는 네 가지의 색상을 사용했습니다.

Matplotlib의 다양한 색상에 대해서는 Matplotlib 색상 지정하기 페이지를 참고하세요.

결과는 아래와 같습니다.

예제2

import matplotlib.pyplot as plt
import numpy as np

y = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]
colors = ['y', 'dodgerblue', 'C2']

plt.barh(y, values, color=colors)
plt.yticks(y, years)

plt.show()
plt.barh() 함수의 color 파라미터에 색상의 이름을 리스트의 형태로 입력하면,

막대의 색상을 각각 다르게 지정할 수 있습니다.

결과는 아래와 같습니다.

막대 높이 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

y = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.barh(y, values, height=0.4)
# plt.barh(y, values, height=0.6)
# plt.barh(y, values, height=0.8)
# plt.barh(y, values, height=1.0)
plt.yticks(y, years)

plt.show()
plt.barh() 함수의 height 파라미터는 막대의 높이를 지정합니다.

예제에서는 막대의 높이를 0.4/0.6/0.8/1.0으로 지정했고, 디폴트는 0.8입니다.

스타일 꾸미기

예제

import matplotlib.pyplot as plt
import numpy as np

y = np.arange(3)
years = ['2018', '2019', '2020']
values = [100, 400, 900]

plt.barh(y, values, align='edge', edgecolor='#eee',
         linewidth=5, tick_label=years)

plt.show()
이번에는 수평 막대 그래프의 테두리의 색, 두께 등 스타일을 적용해 보겠습니다.

align은 눈금과 막대의 위치를 조절합니다. 디폴트는 ‘center’이며, ‘edge’로 설정하면 막대의 아래쪽 끝에 눈금이 표시됩니다.

edgecolor는 막대의 테두리 색, linewidth는 테두리의 두께를 지정합니다.

tick_label을 리스트 또는 어레이 형태로 지정하면, 틱에 문자열을 순서대로 나타낼 수 있습니다.

결과는 아래와 같습니다.

# Matplotlib 산점도 그리기 #

산점도 (Scatter plot)는 두 변수의 상관 관계를 직교 좌표계의 평면에 점으로 표현하는 그래프입니다.

matplotlib.pyplot 모듈의 scatter() 함수를 이용하면 산점도를 그릴 수 있습니다.

Keyword: plt.scatter(), scatter plot, 산점도

기본 사용

예제

import matplotlib.pyplot as plt
import numpy as np

np.random.seed(0)

n = 50
x = np.random.rand(n)
y = np.random.rand(n)

plt.scatter(x, y)
plt.show()
NumPy의 random 모듈에 포함된 rand() 함수를 사용해서 [0, 1) 범위의 난수를 각각 50개씩 생성했습니다.

x, y 데이터를 순서대로 scatter() 함수에 입력하면 x, y 값에 해당하는 위치에 기본 마커가 표시됩니다.

결과는 아래와 같습니다.

색상과 크기 지정하기

예제1

import matplotlib.pyplot as plt
import numpy as np

np.random.seed(0)

n = 50
x = np.random.rand(n)
y = np.random.rand(n)
area = (30 * np.random.rand(n))**2
colors = np.random.rand(n)

plt.scatter(x, y, s=area, c=colors)
plt.show()
scatter() 함수의 s, c 파라미터는 각각 마커의 크기와 색상을 지정합니다.

마커의 크기는 size**2 의 형태로 지정합니다.

예를 들어 plot() 함수에 markersize=20으로 지정하는 것과

scatter() 함수에 s=20**2으로 지정하는 것은 같은 크기의 마커를 표시하도록 합니다.

마커의 색상은 데이터의 길이와 같은 크기의 숫자 시퀀스 또는 rgb, 그리고 Hex code 색상을 입력해서 지정합니다.

마커에 임의의 크기와 색상을 지정했습니다. 결과는 아래와 같습니다.

예제2
plot() 함수의 markersize 지정과 scatter() 함수의 s (size) 지정에 대해서는 아래의 예제를 참고하세요.

import matplotlib.pyplot as plt

plt.plot([1], [1], 'o', markersize=20, c='#FF5733')
plt.scatter([2], [1], s=20**2, c='#33FFCE')

plt.text(0.5, 1.05, 'plot(markersize=20)', fontdict={'size': 14})
plt.text(1.6, 1.05, 'scatter(s=20**2)', fontdict={'size': 14})
plt.axis([0.4, 2.6, 0.8, 1.2])
plt.show()
plot() 함수의 markersize를 20으로, scatter() 함수의 s를 20**2으로 지정했습니다.

아래 그림과 같이 동일한 크기의 마커를 표시합니다.

투명도와 컬러맵 설정하기

예제

import matplotlib.pyplot as plt
import numpy as np

np.random.seed(0)

n = 50
x = np.random.rand(n)
y = np.random.rand(n)
area = (30 * np.random.rand(n))**2
colors = np.random.rand(n)

plt.scatter(x, y, s=area, c=colors, alpha=0.5, cmap='Spectral')
plt.colorbar()
plt.show()
alpha 파라미터는 마커의 투명도를 지정합니다. 0에서 1 사이의 값을 입력합니다.

cmap 파라미터에 컬러맵에 해당하는 문자열을 지정할 수 있습니다.

# Matplotlib 히스토그램 그리기 #
히스토그램 (Histogram)은 도수분포표를 그래프로 나타낸 것으로서, 가로축은 계급, 세로축은 도수 (횟수나 개수 등)를 나타냅니다.

이번에는 matplotlib.pyplot 모듈의 hist() 함수를 이용해서 다양한 히스토그램을 그려 보겠습니다.

Keyword: plt.hist(), histogram, 히스토그램

기본 사용

예제

import matplotlib.pyplot as plt

weight = [68, 81, 64, 56, 78, 74, 61, 77, 66, 68, 59, 71,
          80, 59, 67, 81, 69, 73, 69, 74, 70, 65]

plt.hist(weight)

plt.show()
weight는 몸무게 값을 나타내는 리스트입니다.

hist() 함수에 리스트의 형태로 값들을 직접 입력해주면 됩니다.

결과는 아래와 같습니다.

구간 개수 지정하기
hist() 함수의 bins 파라미터는 히스토그램의 가로축 구간의 개수를 지정합니다.

아래 그림과 같이 구간의 개수에 따라 히스토그램 분포의 형태가 달라질 수 있기 때문에

적절한 구간의 개수를 지정해야 합니다.

예제

import matplotlib.pyplot as plt

weight = [68, 81, 64, 56, 78, 74, 61, 77, 66, 68, 59, 71,
          80, 59, 67, 81, 69, 73, 69, 74, 70, 65]

plt.hist(weight, label='bins=10')
plt.hist(weight, bins=30, label='bins=30')
plt.legend()
plt.show()
첫번째 히스토그램은 bins 값을 지정하지 않아서 기본값인 10으로 지정되었습니다.

두번째 히스토그램은 bins 값을 30으로 지정했습니다.

아래와 같이 다른 분포를 나타냅니다.

누적 히스토그램 그리기

예제

import matplotlib.pyplot as plt

weight = [68, 81, 64, 56, 78, 74, 61, 77, 66, 68, 59, 71,
          80, 59, 67, 81, 69, 73, 69, 74, 70, 65]

plt.hist(weight, cumulative=True, label='cumulative=True')
plt.hist(weight, cumulative=False, label='cumulative=False')
plt.legend(loc='upper left')
plt.show()
cumulative 파라미터를 True로 지정하면 누적 히스토그램을 나타냅니다.

디폴트는 False로 지정됩니다.

히스토그램 종류 지정하기

예제

import matplotlib.pyplot as plt

weight = [68, 81, 64, 56, 78, 74, 61, 77, 66, 68, 59, 71,
        80, 59, 67, 81, 69, 73, 69, 74, 70, 65]
weight2 = [52, 67, 84, 66, 58, 78, 71, 57, 76, 62, 51, 79,
        69, 64, 76, 57, 63, 53, 79, 64, 50, 61]

plt.hist((weight, weight2), histtype='bar')
plt.title('histtype - bar')
plt.figure()

plt.hist((weight, weight2), histtype='barstacked')
plt.title('histtype - barstacked')
plt.figure()

plt.hist((weight, weight2), histtype='step')
plt.title('histtype - step')
plt.figure()

plt.hist((weight, weight2), histtype='stepfilled')
plt.title('histtype - stepfilled')
plt.show()
histtype은 히스토그램의 종류를 지정합니다.

{‘bar’, ‘barstacked’, ‘step’, ‘stepfilled’} 중에서 선택할 수 있으며, 디폴트는 ‘bar’입니다.

예제에서와 같이 두 종류의 데이터를 히스토그램으로 나타냈을 때, histtype의 값에 따라 각기 다른 히스토그램이 그려집니다.

NumPy 난수의 분포 나타내기

예제

import matplotlib.pyplot as plt
import numpy as np


a = 2.0 * np.random.randn(10000) + 1.0
b = np.random.standard_normal(10000)
c = 20.0 * np.random.rand(5000) - 10.0

plt.hist(a, bins=100, density=True, alpha=0.7, histtype='step')
plt.hist(b, bins=50, density=True, alpha=0.5, histtype='stepfilled')
plt.hist(c, bins=100, density=True, alpha=0.9, histtype='step')

plt.show()
Numpy의 np.random.randn(), np.random.standard_normal(), np.random.rand() 함수를 이용해서 임의의 값들을 만들었습니다.

어레이 a는 표준편차 2.0, 평균 1.0을 갖는 정규분포, 어레이 b는 표준정규분포를 따릅니다.

어레이 c는 -10.0에서 10.0 사이의 균일한 분포를 갖는 5000개의 임의의 값입니다.

density=True로 설정해주면, 밀도함수가 되어서 막대의 아래 면적이 1이 됩니다.

alpha는 투명도를 의미합니다. 0.0에서 1.0 사이의 값을 갖습니다.

# Matplotlib 파이 차트 그리기 #

파이 차트 (Pie chart, 원 그래프)는 범주별 구성 비율을 원형으로 표현한 그래프입니다.

위의 그림과 같이 부채꼴의 중심각을 구성 비율에 비례하도록 표현합니다.

matplotlib.pyplot 모듈의 pie() 함수를 이용해서 파이 차트를 그리는 방법에 대해 소개합니다.

기본 사용
예제

import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']

plt.pie(ratio, labels=labels, autopct='%.1f%%')
plt.show()
우선 각 영역의 비율과 이름을 ratio와 labels로 지정해주고,

pie() 함수에 순서대로 입력합니다.

autopct는 부채꼴 안에 표시될 숫자의 형식을 지정합니다. 소수점 한자리까지 표시하도록 설정했습니다.

시작 각도와 방향 설정하기
예제

import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False)
plt.show()
startangle는 부채꼴이 그려지는 시작 각도를 설정합니다.

디폴트는 0도 (양의 방향 x축)로 설정되어 있습니다.

counterclock=False로 설정하면 시계 방향 순서로 부채꼴 영역이 표시됩니다.

중심에서 벗어나는 정도 설정하기
예제

import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']
explode = [0, 0.10, 0, 0.10]

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, explode=explode)
plt.show()
explode는 부채꼴이 파이 차트의 중심에서 벗어나는 정도를 설정합니다.

‘Banana’와 ‘Grapes’ 영역에 대해서 반지름의 10% 만큼 벗어나도록 설정했습니다.

그림자 나타내기
예제

import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']
explode = [0.05, 0.05, 0.05, 0.05]

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, explode=explode, shadow=True)
plt.show()
shadow를 True로 설정하면, 파이 차트에 그림자가 표시됩니다.

색상 지정하기
예제1

import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']
explode = [0.05, 0.05, 0.05, 0.05]
colors = ['silver', 'gold', 'whitesmoke', 'lightgray']

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, explode=explode, shadow=True, colors=colors)
plt.show()
colors를 사용하면 각 영역의 색상을 자유롭게 지정할 수 있습니다.

‘silver’, ‘gold’, ‘lightgray’, ‘whitesmoke’ 등 색상의 이름을 사용해서 각 영역의 색상을 지정했습니다.

결과는 아래와 같습니다.

예제2

import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']
explode = [0.05, 0.05, 0.05, 0.05]
colors = ['#ff9999', '#ffc000', '#8fd9b6', '#d395d0']

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, explode=explode, shadow=True, colors=colors)
plt.show()
Hex code를 이용해서 더욱 다양한 색상을 지정할 수 있습니다.

각 영역의 색상을 ‘#ff9999’, ‘#ffc000’, ‘#8fd9b6’, ‘#d395d0’ 코드로 지정하면 아래와 같이 표시됩니다.

부채꼴 스타일 지정하기
예제

import matplotlib.pyplot as plt

ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']
colors = ['#ff9999', '#ffc000', '#8fd9b6', '#d395d0']
wedgeprops={'width': 0.7, 'edgecolor': 'w', 'linewidth': 5}

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, colors=colors, wedgeprops=wedgeprops)
plt.show()
wedgeprops는 부채꼴 영역의 스타일을 설정합니다.

wedgeprops 딕셔너리의 ‘width’, ‘edgecolor’, ‘linewidth’ 키를 이용해서 각각 부채꼴 영역의 너비 (반지름에 대한 비율),

테두리의 색상, 테두리 선의 너비를 설정했습니다.

결과는 아래와 같습니다.

Matplotlib 히트맵 그리기

히트맵 (Heatmap)은 다양한 값을 갖는 숫자 데이터를 열분포 형태와 같이 색상을 이용해서 시각화한 것입니다.

지도 이미지 위에 인구의 분포를 표현하거나, 웹사이트 이미지 위에 마우스의 클릭 위치를 표시하는 등의 다양한 정보를 시각화할 수 있습니다.

matplotlib.pyplot 모듈의 matshow() 함수를 이용해서 2차원 어레이 형태의 숫자 데이터를 히트맵으로 나타내 보겠습니다.

Keyword: plt.matshow(), heatmap, 히트맵

기본 사용

예제

import matplotlib.pyplot as plt
import numpy as np

arr = np.random.standard_normal((30, 40))

plt.matshow(arr)

plt.show()
np.random.standard_normal() 로 만들어진 2차원 어레이 arr는 표준정규분포를 갖는 (30, 40) 형태의 2차원 어레이 입니다.

matshow() 함수에 어레이의 형태로 값들을 직접 입력하면 아래와 같은 그래프가 표시됩니다.

컬러바 나타내기

예제1

import matplotlib.pyplot as plt
import numpy as np

arr = np.random.standard_normal((30, 40))

plt.matshow(arr)
plt.colorbar()

plt.show()
히트맵에 컬러바를 함께 나타내기 위해서 colorbar() 함수를 사용합니다.

예제2

plt.colorbar(shrink=0.8, aspect=10)
colorbar() 함수의 shrink 파라미터는 컬러바의 크기를 결정합니다.

shrink 파라미터의 디폴트 값은 1.0이며, 예제에서는 0.8로 지정했습니다.

colorbar() 함수의 aspect 파라미터는 컬러바의 종횡비 (Aspect ratio)를 결정합니다.

aspect 파라미터의 디폴트 값은 20이며, 예제에서는 10으로 지정했습니다.

결과는 아래와 같습니다.

색상 범위 지정하기

예제

import matplotlib.pyplot as plt
import numpy as np

arr = np.random.standard_normal((30, 40))

plt.matshow(arr)
plt.colorbar(shrink=0.8, aspect=10)
# plt.clim(-1.0, 1.0)
plt.clim(-3.0, 3.0)

plt.show()
히트맵에 표시될 색상의 범위를 지정하기 위해서 clim() 함수를 사용합니다.

아래 그림은 색상의 범위를 -1.0 ~ 1.0 으로 지정한 히트맵입니다.

arr의 값 중 -1.0 보다 작거나 1.0 보다 큰 값에 대해서는 각각 -1.0, 1.0과 같은 색으로 나타납니다.
아래 그림은 색상의 범위를 -3.0 ~ 3.0 으로 지정한 히트맵입니다.

arr의 값 중 -3.0 보다 작거나 3.0 보다 큰 값에 대해서는 각각 -3.0, 3.0과 같은 색으로 나타납니다.

컬러맵 지정하기
예제

import matplotlib.pyplot as plt
import numpy as np

arr = np.random.standard_normal((30, 40))
# cmap = plt.get_cmap('PiYG')
# cmap = plt.get_cmap('BuGn')
# cmap = plt.get_cmap('Greys')
cmap = plt.get_cmap('bwr')

plt.matshow(arr, cmap=cmap)
plt.colorbar()
plt.show()
cmap 키워드 인자를 통해 표시할 컬러맵의 종류를 지정할 수 있습니다.

matplotlib.pyplot 모듈의 get_cmap() 함수를 이용해서 Matplotlib 컬러맵을 가져와서 matshow()에 입력해줍니다.

다양한 컬러맵을 적용한 히트맵은 아래와 같습니다.

# Matplotlib 그래프 스타일 설정하기 #

Matplotlib 그래프의 스타일을 간단하게 설정하는 방법을 소개합니다.

Matplotlib의 스타일 파라미터 rcParams를 사용해서 그래프를 커스터마이즈할 수 있습니다.

기본 사용

예제

import matplotlib.pyplot as plt

plt.style.use('bmh')
# plt.style.use('ggplot')
# plt.style.use('classic')
# plt.style.use('Solarize_Light2')
# plt.style.use('default')

plt.plot([1, 2, 3, 4], [4, 6, 2, 7])
plt.show()
matplotlib.style 모듈은 미리 만들어놓은 Matplotlib 그래프 스타일을 포함하고 있습니다.

matplotlib.style.use()를 사용해서 다양한 스타일을 지정할 수 있습니다.

예제에서는 ‘bmh’, ‘ggplot’, ‘classic’, ‘Solarize_Light2’ 네 가지의 스타일을 지정했습니다.

다시 기본 스타일로 돌아가기 위해서는 plt.style.use(‘default’)를 호출하면 됩니다.

사용할 수 있는 Matplotlib 스타일을 확인하기 위해서는 아래의 코드를 실행하세요.

import matplotlib.pyplot as plt

print(plt.style.available)

rcParams 사용하기

예제1

import matplotlib.pyplot as plt

plt.style.use('default')
plt.rcParams['figure.figsize'] = (6, 3)
plt.rcParams['font.size'] = 12
# plt.rcParams['figure.figsize'] = (4, 3)
# plt.rcParams['font.size'] = 14

plt.plot([1, 2, 3, 4], [4, 6, 2, 7])
plt.show()
미리 지정해놓은 스타일을 사용하지 않고, 각각의 스타일 관련 파라미터 (rcParams)를 지정할 수 있습니다.

‘figure.figsize’를 사용해서 그래프 이미지의 크기를 (6, 3)과 (4, 3)으로,

‘font.size’를 사용해서 폰트의 크기를 12와 14로 각각 지정했습니다.

아래의 두 그래프를 얻을 수 있습니다.

예제2

import matplotlib.pyplot as plt

plt.style.use('default')
plt.rcParams['lines.linewidth'] = 3
plt.rcParams['lines.linestyle'] = '-'
# plt.rcParams['lines.linewidth'] = 5
# plt.rcParams['lines.linestyle'] = '--'

plt.plot([1, 2, 3, 4], [4, 6, 2, 7])
plt.show()
이번에는 ‘lines.linewidth’를 사용해서 선의 두께를 3과 5로,

‘lines.linestyle’를 사용해서 선의 스타일을 ‘-‘과 ‘- -‘로 지정했습니다.

아래와 같은 그래프가 나타납니다.

예제3

import matplotlib.pyplot as plt

plt.style.use('default')
plt.rcParams['xtick.top'] = True
plt.rcParams['ytick.right'] = True
plt.rcParams['xtick.direction'] = 'in'
plt.rcParams['ytick.direction'] = 'in'
# plt.rcParams['xtick.major.size'] = 7
# plt.rcParams['ytick.major.size'] = 7
# plt.rcParams['xtick.minor.visible'] = True
# plt.rcParams['ytick.minor.visible'] = True

plt.plot([1, 2, 3, 4], [4, 6, 2, 7])
plt.show()
축 눈금과 관련된 rcParams을 설정했습니다.

‘xtick.top’, ‘ytick.right’을 True로 설정하면 데이터 영역의 위, 오른쪽에 눈금이 표시됩니다.

‘xtick.direction’, ytick.direction’은 눈금이 표시될 방향이며, {‘in’, ‘out’, ‘inout’} 중 선택할 수 있습니다.

‘xtick.major.size’, ytick.major.size’는 주 눈금의 크기 (길이)를 지정합니다.

‘xtick.minor.visible’, ytick.minor.visible’을 True로 설정하면 보조 눈금이 함께 표시됩니다.

# Matplotlib 이미지 저장하기 #

matplotlib.pyplot 모듈의 savefig() 함수를 사용해서 그래프를 이미지 파일 등으로 저장하는 방법을 소개합니다.

기본 사용

예제

import numpy as np
import matplotlib.pyplot as plt

x1 = np.linspace(0.0, 5.0)
x2 = np.linspace(0.0, 2.0)

y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)
y2 = np.cos(2 * np.pi * x2)

plt.subplot(2, 1, 1)                # nrows=2, ncols=1, index=1
plt.plot(x1, y1, 'o-')
plt.title('1st Graph')
plt.ylabel('Damped oscillation')

plt.subplot(2, 1, 2)                # nrows=2, ncols=1, index=2
plt.plot(x2, y2, '.-')
plt.title('2nd Graph')
plt.xlabel('time (s)')
plt.ylabel('Undamped')

plt.tight_layout()
# plt.show()
plt.savefig('savefig_default.png')

plt.savefig() 함수에 파일 이름을 입력해주면 아래와 같은 이미지 파일이 저장됩니다.

dpi 설정하기

예제

plt.savefig('savefig_default.png')
plt.savefig('savefig_50dpi.png', dpi=50)
plt.savefig('savefig_200dpi.png', dpi=200)
dpi (Dots per Inch)는 이미지의 해상도를 설정합니다. 디폴트는 dpi=100입니다.

해상도에 따라 아래와 같은 이미지가 저장됩니다.

facecolor 설정하기

예제

plt.savefig('savefig_facecolor.png', facecolor='#eeeeee')
facecolor는 이미지의 배경색을 설정합니다.

아래와 같은 이미지가 저장됩니다.

edgecolor 설정하기

예제

import numpy as np
import matplotlib.pyplot as plt

plt.figure(linewidth=2)

x1 = np.linspace(0.0, 5.0)
x2 = np.linspace(0.0, 2.0)

y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)
y2 = np.cos(2 * np.pi * x2)

plt.subplot(2, 1, 1)                # nrows=2, ncols=1, index=1
plt.plot(x1, y1, 'o-')
plt.title('1st Graph')
plt.ylabel('Damped oscillation')

plt.subplot(2, 1, 2)                # nrows=2, ncols=1, index=2
plt.plot(x2, y2, '.-')
plt.title('2nd Graph')
plt.xlabel('time (s)')
plt.ylabel('Undamped')

# plt.show()
plt.savefig('savefig_edgecolor.png', facecolor='#eeeeee', edgecolor='blue')
edgecolor는 이미지의 테두리선의 색상을 설정합니다.

테두리선의 너비가 기본적으로 0이기 때문에 먼저 linewidth=2로 설정해 준 다음 테두리 색을 지정합니다.

아래와 같은 이미지가 저장됩니다.

bbox_inches 설정하기

예제

plt.savefig('savefig_bbox_inches.png', facecolor='#eeeeee')
plt.savefig('savefig_bbox_inches2.png', facecolor='#eeeeee', bbox_inches='tight')
bbox_inches는 그래프로 저장할 영역을 설정합니다.

디폴트로 None이지만, ‘tight’로 지정하면 여백을 최소화하고 그래프 영역만 이미지로 저장합니다.

pad_inches 설정하기

예제

plt.savefig('savefig_pad_inches.png', facecolor='#eeeeee',
            bbox_inches='tight', pad_inches=0.3)
plt.savefig('savefig_pad_inches2.png', facecolor='#eeeeee',
            bbox_inches='tight', pad_inches=0.5)
bbox_inches=’tight’로 지정하면 pad_inches를 함께 사용해서 여백 (Padding)을 지정할 수 있습니다.

pad_inches의 디폴트 값은 0.1이며, 0.3과 0.5로 지정했을 때의 결과는 아래와 같습니다.

# Matplotlib 객체 지향 인터페이스 1 #

지금까지 matplotlib.pyplot 모듈의 다양한 함수들을 이용해서 간편하게 그래프를 그렸습니다.

Matplotlib는 그래프를 다루는 두 가지의 인터페이스를 제공하는데 첫번째는 MATLAB 스타일로 pyplot 모듈을 사용하는 방식이고, 두번째는 객체 지향 인터페이스입니다.

Matplotlib 공식 문서에 의하면 더욱 커스터마이즈된 그래프를 위해 객체 지향 인터페이스를 사용하기를 권장합니다.

plt.subplots() 사용하기
예제1

import matplotlib.pyplot as plt

fig, ax = plt.subplots()
plt.show()
matplotlib.pyplot 모듈은 subplots()라는 유용한 함수를 제공합니다. (matplotlib.pyplot.subplots)

subplots() 함수를 호출하면 figure (fig)과 subplot (ax) 객체를 생성해서 튜플의 형태로 반환합니다.

아래와 같은 그림이 나타납니다.

예제2

import matplotlib.pyplot as plt

# fig, ax = plt.subplots()
fig = plt.figure()
ax = fig.add_axes([0, 0, 1, 1])

plt.show()
fig, ax = plt.subplots()과 같이 사용하지 않고 이 예제와 같이 사용할 수도 있습니다.

plt.figure()는 Figure 클래스의 인스턴스를 반환합니다.

Figure 클래스의 인스턴스 fig의 메서드 add_axes()는 fig에 axes를 하나 추가합니다.

add_axes([left, bottom, width, height])의 형태로 0에서 1 사이의 값을 입력합니다.

이 페이지의 예제들에서는 plt.subplots() 함수를 사용합니다.

행과 열 설정하기 (nrows, ncols)
예제

import matplotlib.pyplot as plt

fig, ax = plt.subplots(2, 2)
plt.show()
plt.subplots(nrows, ncols)의 형태로 행과 열의 개수를 지정할 수 있습니다.

만약 지정하지 않으면 기본적으로 행과 열의 개수는 모두 1입니다.

결과는 아래와 같습니다.

X, Y축 공유하기 (sharex, sharey)
앞의 그림에서 네 개의 그래프 영역은 X, Y축의 범위가 같습니다.

이 경우에 중복해서 표시하지 않도록 X축 또는 Y축을 공유하도록 설정할 수 있습니다.

예제

import matplotlib.pyplot as plt

fig, ax = plt.subplots(2, 2, sharex=True, sharey=True)
plt.show()
sharex=True, sharey=True로 설정함으로써 아래와 같이 중복된 축을 한번만 표시하도록 했습니다.

sharex, sharey에 True, False 이 외에도 ‘all’, ‘none’, ‘row’, ‘col’ 등을 지정할 수 있습니다.

결과는 아래와 같습니다.

# Matplotlib 객체 지향 인터페이스 2 #

그래프 그리기
예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1, 5)     # [1, 2, 3, 4]

fig, ax = plt.subplots(2, 2, sharex=True, sharey=True, squeeze=True)
ax[0][0].plot(x, np.sqrt(x))      # left-top
ax[0][1].plot(x, x)               # right-top
ax[1][0].plot(x, -x+5)            # left-bottom
ax[1][1].plot(x, np.sqrt(-x+5))   # right-bottom

plt.show()
NumPy를 이용해서 x값들을 만들고, 네 개의 그래프 영역에 각각 다른 y값을 그래프로 나타냈습니다.

plt.subplots()이 반환하는 ax는 Matplotlib의 Axes 클래스의 인스턴스입니다.

행과 열을 각각 2, 2로 지정했기 때문에 ax는 2×2의 형태를 갖는 NumPy 어레이가 됩니다.

위치에 따라 각각 ax[0][0], ax[0][1], ax[1][0], ax[1][1]과 같이 접근해서 사용할 수 있습니다.

스타일 설정하기
예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1, 5)     # [1, 2, 3, 4]

fig, ax = plt.subplots(2, 2, sharex=True, sharey=True, squeeze=True)
ax[0][0].plot(x, np.sqrt(x), 'gray', linewidth=3)
ax[0][1].plot(x, x, 'g^-', markersize=10)
ax[1][0].plot(x, -x+5, 'ro--')
ax[1][1].plot(x, np.sqrt(-x+5), 'b.-.')

plt.show()
plot()에 각각의 그래프의 스타일을 커스터마이즈하도록 설정할 수 있습니다.

(자세한 내용은 마커 지정하기, 색상 지정하기 페이지를 참고하세요.)

제목과 범례 표시하기
예제

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1, 5)     # [1, 2, 3, 4]

fig, ax = plt.subplots(2, 2, sharex=True, sharey=True, squeeze=True)
ax[0][0].plot(x, np.sqrt(x), 'gray', linewidth=3, label='y=np.sqrt(x)')
ax[0][0].set_title('Graph 1')
ax[0][0].legend()
ax[0][1].plot(x, x, 'g^-', markersize=10, label='y=x')
ax[0][1].set_title('Graph 2')
ax[0][1].legend(loc='upper left')
ax[1][0].plot(x, -x+5, 'ro--', label='y=-x+5')
ax[1][0].set_title('Graph 3')
ax[1][0].legend(loc='lower left')
ax[1][1].plot(x, np.sqrt(-x+5), 'b.-.', label='y=np.sqrt(-x+5)')
ax[1][1].set_title('Graph 4')
ax[1][1].legend(loc='upper center')

plt.show()
set_title()과 legend()를 이용해서 각각의 그래프에 제목과 범례를 설정할 수 있습니다.

set_title()은 입력한 문자열을 그래프의 제목으로 나타냅니다.

legend()는 plot()에서 label을 이용해서 지정한 문자열을 범례에 나타냅니다.

그래프 영역에서 범례가 표시될 위치를 지정할 수 있는데, 지정하지 않으면 최적의 위치에 범례를 표시합니다.

범례가 표시될 위치를 설정하기 위한 옵션은 아래와 같습니다.

'best'
'upper right'
'upper left'
'lower left'
'lower right'
'right'
'center left'
'center right'
'lower center'
'upper center'
'center'

# Matplotlib 축 위치 조절하기 #

Matplotlib의 기본적인 레이아웃은 그래프 상하좌우의 네 방향에 데이터 영역을 나타내는 직선이 그려지는 형태입니다.

이 페이지에서는 데이터 영역을 나타내는 직선을 필요한 위치에 선택적으로 표시하는 방법에 대해 소개합니다.

기본 사용
 

예제

import matplotlib.pyplot as plt
import numpy as np

plt.style.use('default')
plt.rcParams['figure.figsize'] = (6, 3)
plt.rcParams['font.size'] = 12


fig, ax = plt.subplots()

ax.set_title('Mean Squared Error', pad=20)
ax.set_xlim(-3, 3)
ax.set_ylim(0, 3)
ax.set_xticks([-3, -2, -1, 0, 1, 2, 3])
ax.set_yticks([1, 2, 3])

ax.spines['left'].set_position('center')        # 왼쪽 축을 가운데 위치로 이동
ax.spines['right'].set_visible(False)          # 오른쪽 축을 보이지 않도록
ax.spines['top'].set_visible(False)            # 위 축을 보이지 않도록
ax.spines['bottom'].set_position(('data', 0))   # 아래 축을 데이터 0의 위치로 이동
ax.tick_params('both', length=0)                # Tick의 눈금 길이 0

x = np.linspace(-3, 3, 100)
ax.set_xlabel('y_true - y_pred', fontdict={'fontsize': 14}, labelpad=10)
ax.plot(x, x**2, color='#4799FF', linewidth=5)

plt.show()
ax.spine[‘left’]는 왼쪽 축의 spine을 가리키는 클래스입니다.

Matplotlib의 spine은 데이터 영역의 경계를 나타내는 선을 말합니다.

이 spine 클래스의 set_position() 메서드를 사용하면 이 직선의 위치를 조절할 수 있습니다.

set_position(‘center’)과 같이 데이터 영역의 가운데에 위치시키거나,

set_position((‘data’, 0))과 같이 특정 데이터 좌표의 위치에 직선을 표시할 수 있습니다.

set_visible(False)는 spine이 그래프에 표시되지 않도록 합니다.

아래 그림은 위 예제의 강조된 부분을 적용하지 않은 그래프이고,

# Matplotlib 이중 Y축 표시하기 #
두 종류의 데이터를 동시에 하나의 그래프에 표시하기 위해 이중 축을 표시할 수 있습니다.

이 페이지에서는 Matplotlib 그래프에 두 개의 축을 동시에 표시하는 방법에 대해 소개합니다.

Keyword: Matplotlib.axes.Axes.twinx(), double y-axis, 이중 Y축, get_label()

기본 사용

예제

import matplotlib.pyplot as plt
import numpy as np

plt.style.use('default')
plt.rcParams['figure.figsize'] = (4, 3)
plt.rcParams['font.size'] = 12

x = np.arange(0, 3)
y1 = x + 1
y2 = -x - 1

fig, ax1 = plt.subplots()
ax1.plot(x, y1, color='green')

ax2 = ax1.twinx()
ax2.plot(x, y2, color='deeppink')

plt.show()
ax1.plot(x, y1, color=’green’)은 첫번째 축에 (x, y1) 데이터를 나타냅니다.

ax1.twinx()는 ax1과 x축을 공유하는 새로운 Axes 객체를 만듭니다.

ax2.plot(x, y2)는 새로운 Axes 객체에 (x, y2) 데이터를 나타냅니다.

결과는 아래와 같습니다.

축 레이블 표시하기

예제

import matplotlib.pyplot as plt
import numpy as np

plt.style.use('default')
plt.rcParams['figure.figsize'] = (4, 3)
plt.rcParams['font.size'] = 12

x = np.arange(0, 3)
y1 = x + 1
y2 = -x - 1

fig, ax1 = plt.subplots()
ax1.set_xlabel('X-Axis')
ax1.set_ylabel('1st Y-Axis')
ax1.plot(x, y1, color='green')

ax2 = ax1.twinx()
ax2.set_ylabel('2nd Y-Axis')
ax2.plot(x, y2, color='deeppink')

plt.show()
set_xlabel(), set_ylabel() 메서드는 각 축에 대한 레이블을 표시하도록 합니다.

(Matplotlib 축 레이블 설정하기 페이지를 참고하세요.)

결과는 아래와 같습니다.

범례 표시하기 1

예제

import matplotlib.pyplot as plt
import numpy as np

plt.style.use('default')
plt.rcParams['figure.figsize'] = (4, 3)
plt.rcParams['font.size'] = 14

x = np.arange(0, 3)
y1 = x + 1
y2 = -x - 1

fig, ax1 = plt.subplots()
ax1.set_xlabel('X-Axis')
ax1.set_ylabel('1st Y-Axis')
ax1.plot(x, y1, color='green', label='1st Data')
ax1.legend(loc='upper right')

ax2 = ax1.twinx()
ax2.set_ylabel('2nd Y-Axis')
ax2.plot(x, y2, color='deeppink', label='2nd Data')
ax2.legend(loc='lower right')

plt.show()
각 축의 데이터 곡선에 대한 범례를 나타내기 위해 legend() 메서드를 사용합니다.

(Matplotlib 범례 표시하기 페이지를 참고하세요.)

결과는 아래와 같습니다.

범례 표시하기 2

예제

import matplotlib.pyplot as plt
import numpy as np

plt.style.use('default')
plt.rcParams['figure.figsize'] = (4, 3)
plt.rcParams['font.size'] = 14

x = np.arange(0, 3)
y1 = x + 1
y2 = -x - 1

fig, ax1 = plt.subplots()
ax1.set_xlabel('X-Axis')
ax1.set_ylabel('1st Y-Axis')
line1 = ax1.plot(x, y1, color='green', label='1st Data')

ax2 = ax1.twinx()
ax2.set_ylabel('2nd Y-Axis')
line2 = ax2.plot(x, y2, color='deeppink', label='2nd Data')

lines = line1 + line2
labels = [l.get_label() for l in lines]
ax1.legend(lines, labels, loc='upper right')
plt.show()
두 축에 대한 범례를 하나의 텍스트 상자에 표시하기 위해서는 위의 예제와 같이

두 곡선을 먼저 합친 후 legend() 메서드를 사용하세요.

결과는 아래와 같습니다.

# Matplotlib 두 종류의 그래프 그리기 #
앞에서 소개한 이중 Y축 표시하기를 이용해서 두 종류의 그래프를 하나의 그래프 영역에 표현해보겠습니다.

Keyword: 두 종류의 그래프, set_zorder(), zorder

기본 사용
 

예제

import matplotlib.pyplot as plt
import numpy as np

# 1. 기본 스타일 설정
plt.style.use('default')
plt.rcParams['figure.figsize'] = (4, 3)
plt.rcParams['font.size'] = 12

# 2. 데이터 준비
x = np.arange(2020, 2027)
y1 = np.array([1, 3, 7, 5, 9, 7, 14])
y2 = np.array([1, 3, 5, 7, 9, 11, 13])

# 3. 그래프 그리기
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
우선 ax1.twinx()로 X축을 공유하는 이중 Y축을 만들고,

ax1.plot()과 ax2.bar()를 사용해서 y1, y2 데이터를 각각 꺾은선 그래프와 막대 그래프의 형태로 나타냈습니다.

(두번째 Y축의 레이블에 표현한 수학적 표현의 사용에 대해서는 링크를 참고하세요.)

결과는 아래와 같습니다.

그래프 순서 지정하기
위의 그림을 보면 녹색의 꺾은선 그래프가 막대의 뒤에 그려져 있어서 잘 보이지 않습니다.

아래 예제에서는 set_zorder() 메서드를 사용해서 그래프가 표시될 순서를 지정해 보겠습니다.

예제

import matplotlib.pyplot as plt
import numpy as np

# 1. 기본 스타일 설정
plt.style.use('default')
plt.rcParams['figure.figsize'] = (4, 3)
plt.rcParams['font.size'] = 12

# 2. 데이터 준비
x = np.arange(2020, 2027)
y1 = np.array([1, 3, 7, 5, 9, 7, 14])
y2 = np.array([1, 3, 5, 7, 9, 11, 13])

# 3. 그래프 그리기
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

ax1.set_zorder(ax2.get_zorder() + 10)
ax1.patch.set_visible(False)

ax1.legend(loc='upper left')
ax2.legend(loc='upper right')

plt.show()
set_zorder() 메서드는 z-축 방향의 순서를 지정합니다.

아래 그림과 같이 zorder가 낮을수록 먼저 그려지고, zorder가 높을수록 나중에 그려집니다.

우선 ax2.get_zorder() 메서드를 사용해서 ax2의 zorder를 얻고,

ax2의 zorder보다 큰 값을 ax1의 zorder로 지정함으로써 그래프가 항상 나중에 그려지도록 설정했습니다.

결과는 아래와 같습니다.


 

