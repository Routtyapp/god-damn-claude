# god-damn-claude

Claude Code 플러그인 모음 — 웹 디자인, 데이터 시각화.

---

## 설치

설치는 **Claude Code TUI 슬래시 커맨드** 또는 **터미널 CLI** 중 하나로 한다.

**1단계 — 마켓플레이스 등록**

```
# TUI (Claude Code 내부)
/plugin marketplace add Baalzebul/god-damn-claude

# CLI (터미널)
claude plugin marketplace add Baalzebul/god-damn-claude
```

**2단계 — 플러그인 설치**

```
# TUI
/plugin install god-damn-design@god-damn-claude
/plugin install god-damn-visualization@god-damn-claude

# CLI
claude plugin install god-damn-design@god-damn-claude
claude plugin install god-damn-visualization@god-damn-claude
```

`/plugin` → **Discover** 탭에서 GUI로 설치해도 된다.

**로컬 경로에서 설치**

```
/plugin marketplace add ./path/to/god-damn-claude
```

---

## god-damn-design (디자인)

| 커맨드 | 설명 |
|--------|------|
| `/god-damn-design:component-usage` | UI 컴포넌트 선택 기준 및 사용 패턴 |
| `/god-damn-design:components-design` | 고급 UI 패턴 구현 (Masonry 등) |
| `/god-damn-design:design-system` | 디자인 토큰, CSS 변수, 테마/다크모드 |
| `/god-damn-design:design-texture` | Glassmorphism, Grainy Gradient, Metallic 등 표면 효과 |
| `/god-damn-design:interactive-design` | CSS 애니메이션, SVG 조명, 커서 인터랙션 |
| `/god-damn-design:pattern-design` | CSS/SVG 기반 반복 패턴 배경 |
| `/god-damn-design:gestalt-rules` | 근접성·유사성 등 게슈탈트 원리 레이아웃 적용 |
| `/god-damn-design:web-grid` | 4px / 12 Column 반응형 그리드 시스템 |
| `/god-damn-design:ui-spec` | 프레임워크 무관 공통 UI 규격 |
| `/god-damn-design:naming-rules` | 파일·레이어·심볼·에셋 네이밍 규칙 |
| `/god-damn-design:css-trig` | CSS 삼각함수 기반 레이아웃 |
| `/god-damn-design:color-interpolation` | OKLCH 기반 색상 보간 |
| `/god-damn-design:micro-interaction` | 버튼·토글·폼 요소 피드백 애니메이션 |
| `/god-damn-design:scroll-animation` | 스크롤 연동 reveal, parallax, scroll-driven |
| `/god-damn-design:3d-transform` | 카드 플립, tilt, translateZ 깊이감 레이어 |
| `/god-damn-design:text-animation` | 타이프라이터, scramble, 글자별 reveal, 그라디언트 텍스트 |

---

## god-damn-visualization (시각화)

| 커맨드 | 설명 |
|--------|------|
| `/god-damn-visualization:line-chart` | 라인 차트 |
| `/god-damn-visualization:bar-chart` | 막대 차트 |
| `/god-damn-visualization:pie-chart` | 파이 차트 |
| `/god-damn-visualization:histogram` | 히스토그램 |
| `/god-damn-visualization:scatter-plot` | 산점도 |
| `/god-damn-visualization:heatmap` | 히트맵 |
| `/god-damn-visualization:area-chart` | 영역 차트 |
| `/god-damn-visualization:box-plot` | 박스 플롯 |
| `/god-damn-visualization:subplot` | 서브플롯 (다중 그래프) |
| `/god-damn-visualization:dual-axis` | 이중 축 차트 |
| `/god-damn-visualization:diagram` | 플로우차트·조직도·타임라인 |
| `/god-damn-visualization:style-guide` | 테마·폰트·색상 설정 |
