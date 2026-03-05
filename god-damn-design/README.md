# god-damn-design

웹디자인 전용 Skills · Agents 모음 플러그인. UI 컴포넌트, 반응형 레이아웃, 디자인 시스템, 표면 효과, 애니메이션 구현 가이드를 제공한다.

---

## 설치

`god-damn-claude` 마켓플레이스를 통해 설치한다.

```
# TUI (Claude Code 내부)
/plugin marketplace add Routtyapp/god-damn-claude
/plugin install god-damn-design@god-damn-claude

# CLI (터미널)
claude plugin marketplace add Routtyapp/god-damn-claude
claude plugin install god-damn-design@god-damn-claude
```

---

## Skills

| 커맨드 | 설명 |
|--------|------|
| `/god-damn-design:component-usage` | UI 컴포넌트 선택 기준 및 사용 패턴 (Mantine / shadcn) |
| `/god-damn-design:components-design` | 고급 UI 패턴 구현 (Masonry 등) |
| `/god-damn-design:design-system` | 디자인 토큰, CSS 변수, 테마/다크모드 |
| `/god-damn-design:design-texture` | Glassmorphism, Grainy Gradient, Horizon Lines, Metallic, Smoky Shader |
| `/god-damn-design:interactive-design` | CSS 애니메이션, SVG 조명, 커서 인터랙션 |
| `/god-damn-design:micro-interaction` | 버튼·토글·폼 요소 피드백 애니메이션 |
| `/god-damn-design:scroll-animation` | 스크롤 연동 reveal, parallax, scroll-driven |
| `/god-damn-design:3d-transform` | 카드 플립, tilt, translateZ 깊이감 레이어 |
| `/god-damn-design:text-animation` | 타이프라이터, scramble, 글자별 reveal, 그라디언트 텍스트 |
| `/god-damn-design:pattern-design` | CSS/SVG 기반 반복 패턴 배경 (renderPattern 커스텀) |
| `/god-damn-design:gestalt-rules` | 근접성·유사성 등 게슈탈트 원리 레이아웃 적용 |
| `/god-damn-design:web-grid` | 4px / 12 Column 반응형 그리드, Hero·Logo·CTA(Score Card / Carousel / Accordion Gallery)·Bento 섹션 패턴 |
| `/god-damn-design:ui-spec` | 프레임워크 무관 공통 UI 규격 |
| `/god-damn-design:naming-rules` | 파일·레이어·심볼·에셋 네이밍 규칙 |
| `/god-damn-design:css-trig` | CSS 삼각함수 기반 레이아웃 (원형 배치, 파동 등) |
| `/god-damn-design:color-interpolation` | OKLCH 기반 색상 보간 |

---

## Agents

Claude가 작업 맥락에 따라 자동으로 호출하는 전문 에이전트.

| 에이전트 | 호출 시점 |
|----------|-----------|
| `design-system-builder` | 디자인 토큰 정의, CSS 변수 체계, 테마/다크모드 설정 |
| `ui-designer` | UI 컴포넌트·레이아웃 구현, 고급 UI 패턴 |
| `visual-designer` | 기존 UI에 시각 효과·텍스처·애니메이션 추가 |

---

## 디렉토리 구조

```
god-damn-design/
├── .claude-plugin/
│   └── plugin.json
├── agents/
│   ├── design-system-builder.md
│   ├── ui-designer.md
│   └── visual-designer.md
└── skills/
    ├── component-usage/
    ├── components-design/
    ├── css-trig/
    ├── color-interpolation/
    ├── design-system/
    ├── design-texture/
    ├── gestalt-rules/
    ├── interactive-design/
    ├── micro-interaction/
    ├── naming-rules/
    ├── pattern-design/
    ├── scroll-animation/
    ├── 3d-transform/
    ├── text-animation/
    ├── ui-spec/
    └── web-grid/
```

---

## 라이선스

MIT License
