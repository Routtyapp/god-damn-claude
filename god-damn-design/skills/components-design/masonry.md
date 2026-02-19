1. masonry 기본 컨테이너

첫 번째 열 기반 메이슨리 컨테이너를 만들려면 display:masonry를 사용하고 grid-template-columns를 사용하여 열 크기를 정의합니다. masonry-direction은 기본적으로 column이므로 이 속성을 설정하지 않아도 됩니다.

.masonry {
  display: masonry;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 10px;
}

대신 행 기반 메이슨리 컨테이너의 경우 display:masonry를 사용하고 grid-template-rows를 사용하여 행 크기를 정의한 다음 masonry-direction:row를 설정합니다.

.masonry {
 display: masonry;
 masonry-direction: row;
 grid-template-rows: repeat(auto-fit, minmax(160px, 1fr));
 gap: 10px;
}

2. 기본 트랙 크기 사용

새 디스플레이 유형을 사용하면 속성 기본값을 다시 생각해 볼 수 있습니다.

그리드에서 grid-template-columns 및 grid-template-rows은 none로 기본 설정되며, 현재 정의된 대로 일반적으로 단일 열 또는 행이 됩니다. masonry의 경우 이 기본값은 바람직하지 않은 레이아웃을 초래하는 경우가 많습니다.

Chromium의 구현에서는 none에 새로 제안된 정의를 추가합니다. 이 정의는 CSS masonry의 기본 트랙 크기를 대체합니다. 이 새로운 기본 트랙 크기는 repeat(auto-fill, auto) 값입니다. 이 값은 트랙 크기를 전혀 정의하지 않고도 멋진 메이슨리 레이아웃을 만듭니다.

.masonry {
  display: masonry;
  gap: 10px;
}

CSS 그리드를 사용하면 트랙의 크기가 조정되기 전에 모든 항목이 배치되므로 이 트랙 자동 크기 조절 정의는 불가능합니다. 하지만 CSS masonry를 사용하면 배치와 크기 조정이 서로 얽혀 있고 간소화되므로 이 제한이 더 이상 적용되지 않습니다. 이 제한이 해제되면 masonry 레이아웃에 더 유용한 트랙 기본 크기를 제공할 수 있습니다.

3. 트랙 걸치기

메이슨리에서는 필요한 경우 여러 트랙에 걸쳐 있을 수 있으므로 항목이 배치된 트랙으로 제한되지 않습니다. 일부 항목이 다른 항목보다 중요하고 더 많은 공간이 필요한 경우에 매우 유용합니다.

예를 들면 다음과 같습니다.

.masonry {
  display: masonry;
  masonry: repeat(auto-fill, minmax(12rem, 1fr));
}

.important-item {
  grid-column: span 2;
}

이 기능을 사용하여 여러 트랙에 걸쳐 특정 항목을 컨테이너 전체 길이로 만들 수도 있습니다.

.masonry {
 display: masonry;
 masonry: repeat(auto-fill, minmax(12rem, 1fr));
}

.full-bleed {
 grid-column: 1 / -1;
}

4. masonry 항목 배치 미세 조정

CSS Masonry에서는 항목이 실행 위치가 가장 짧은 열이나 행에 배치됩니다.

2열 masonry 컨테이너를 상상해 보세요. 컨테이너의 첫 번째 열에 높이가 110px인 항목이 있고 두 번째 열에 높이가 100px인 항목이 있는 경우 세 번째 항목은 두 번째 열에 배치되며, 여기서는 석조 방향의 시작에 10px 더 가까워집니다.

실행 위치의 10px 차이가 사례에 충분히 작다고 생각하고 소스 순서와 더 잘 일치하도록 세 번째 항목을 첫 번째 열에 배치하려면 item-tolerance 속성을 사용하세요.

새로운 item-tolerance 속성은 항목 배치 시 민감도를 제어합니다.

앞의 예시에서는 item-tolerance: 10px;를 컨테이너에 적용하여 상품 배치 변동성을 맞춤설정할 수 있습니다.

.masonry {
  display: masonry;
  masonry: 200px 200px;
  gap: 10px;
  item-tolerance: 10px;
}

초안 사양에서는 이 속성을 item-slack이라고 합니다. CSS 작업 그룹은 최근에 item-tolerance을 이름으로 사용하기로 결정했으며 사양이 곧 업데이트될 예정입니다.

