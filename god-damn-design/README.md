# god-damn-claude

웹디자인 전용 Skills 모음 플러그인입니다.

## 설치

이 플러그인을 Claude Code 계정 단위로 설치하려면:

```bash
# 플러그인 디렉토리로 이동
cd god-damn-claude

# Claude Code에서 플러그인 로드
claude --plugin-dir .
```

또는 글로벌 설정에 추가:
```bash
# ~/.claude/settings.json에 추가
{
  "pluginDirectories": ["/path/to/god-damn-claude"]
}
```

## Skills 목록

### 1. component-builder
UI 컴포넌트 생성 및 설계 가이드

```
/god-damn-claude:component-builder
```

**기능:**
- TypeScript 컴포넌트 베스트 프랙티스
- 컴포넌트 라이브러리 선택 가이드 (Mantine, shadcn, MUI 등)
- Props 설계, 상태 관리, 이벤트 핸들링 패턴
- 접근성 및 성능 최적화

### 2. responsive-layout
반응형 레이아웃 설계 가이드

```
/god-damn-claude:responsive-layout
```

**기능:**
- 브레이크포인트 전략
- Flexbox/Grid 레이아웃 패턴
- Mobile-first vs Desktop-first 접근법
- Container Queries 활용법
- 반응형 타이포그래피

### 3. design-system
디자인 시스템 구축 가이드

```
/god-damn-claude:design-system
```

**기능:**
- 디자인 토큰 정의 (색상, 스페이싱, 타이포그래피)
- CSS 변수 기반 토큰 시스템
- Tailwind CSS 설정
- 테마 시스템 및 다크모드 지원

## 사용 예시

### Button 컴포넌트 생성
```
/god-damn-claude:component-builder Button

TypeScript 기준으로 재사용 가능한 Button 컴포넌트 API를 설계해줘.
variant와 size 타입을 안전하게 제한해줘.
```

### 반응형 그리드 설계
```
/god-damn-claude:responsive-layout grid

3열 카드 그리드를 모바일에서는 1열로 표시하고 싶어.
auto-fit을 사용해서 구현해줘.
```

### 디자인 토큰 설정
```
/god-damn-claude:design-system tokens

프로젝트의 색상 시스템을 CSS 변수로 정의해줘.
다크모드도 지원해야 해.
```

## 디렉토리 구조

```
god-damn-claude/
├── .claude-plugin/
│   └── plugin.json              # 플러그인 메타데이터
├── skills/
│   ├── component-builder/       # UI 컴포넌트 생성
│   │   ├── SKILL.md
│   │   └── patterns/
│   │       └── typescript.md
│   ├── responsive-layout/       # 반응형 레이아웃 설계
│   │   ├── SKILL.md
│   │   └── breakpoints.md
│   └── design-system/           # 디자인 시스템 구축
│       ├── SKILL.md
│       ├── tokens.md
│       └── typography.md
└── README.md
```

## 라이선스

MIT License
