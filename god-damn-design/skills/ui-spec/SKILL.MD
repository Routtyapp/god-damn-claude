---
name: ui-spec
description: 프레임워크 무관 공통 UI 규격서. MUI, shadcn, Mantine 등 어떤 라이브러리를 사용하더라도 일관된 UI 품질을 유지하기 위한 기준을 제공한다.
---

# UI Specification (Framework-Agnostic)
> 목적: MUI / shadcn / Mantine 등 어떤 UI 라이브러리를 사용하더라도  
> 동일한 결과 품질과 일관성을 유지하기 위한 공통 UI 규격서

---

# 0. 설계 원칙

1. **Semantic First**
   - 색상/간격/상태는 “값”이 아니라 “의미”로 정의한다.
   - 예: `#2563eb` ❌ → `brand.primary` ⭕

2. **App-Level Component Only**
   - 앱 코드에서 외부 UI 라이브러리를 직접 import 금지
   - 반드시 `AppButton`, `AppInput` 등 표준 컴포넌트만 사용

3. **Server First**
   - UI는 Server Component 기본
   - 상호작용이 필요한 경우에만 `"use client"`

4. **Predictable UX**
   - Loading / Error / Empty / Success 패턴은 화면마다 동일

---

# 1. Design Tokens (Semantic)

## 1.1 Color Tokens

### Foreground
- `fg.primary`
- `fg.secondary`
- `fg.muted`
- `fg.inverse`
- `fg.danger`
- `fg.success`
- `fg.warning`

### Background
- `bg.canvas`
- `bg.surface`
- `bg.elevated`
- `bg.overlay`
- `bg.brand`
- `bg.danger`

### Border
- `border.default`
- `border.muted`
- `border.focus`
- `border.danger`

### Brand
- `brand.primary`
- `brand.primaryHover`
- `brand.primaryActive`

---

## 1.2 Spacing Scale

단위는 팀에서 고정 (예: 4px 기준)

- `space.0`
- `space.1`
- `space.2`
- `space.3`
- `space.4`
- `space.6`
- `space.8`
- `space.10`
- `space.12`
- `space.16`

---

## 1.3 Radius

- `radius.sm`
- `radius.md`
- `radius.lg`
- `radius.xl`

---

## 1.4 Shadow

- `shadow.0`
- `shadow.1`
- `shadow.2`
- `shadow.3`

---

## 1.5 Typography

### Font Size
- `text.xs`
- `text.sm`
- `text.md`
- `text.lg`
- `text.xl`

### Heading
- `heading.sm`
- `heading.md`
- `heading.lg`
- `heading.xl`

### Weight
- `font.regular`
- `font.medium`
- `font.semibold`
- `font.bold`

---

## 1.6 Breakpoints

- `bp.sm`
- `bp.md`
- `bp.lg`
- `bp.xl`

---

# 2. Component Contracts (App-Level Components)

> 모든 UI는 아래 계약(Contract)을 따른다.

---

## 2.1 AppButton

### Props

- `variant: "solid" | "outline" | "ghost" | "link"`
- `tone: "brand" | "neutral" | "danger"`
- `size: "sm" | "md" | "lg"`
- `loading?: boolean`
- `disabled?: boolean`
- `leftIcon?: ReactNode`
- `rightIcon?: ReactNode`
- `fullWidth?: boolean`
- `data-testid?: string`

### 규칙

- `loading=true` → 클릭 불가 + aria-busy
- `danger`는 파괴적 액션에만 사용
- Primary 액션은 한 화면에 하나만 권장

---

## 2.2 AppInput / AppTextarea

### Props

- `label?: string`
- `value: string`
- `onChange`
- `placeholder?: string`
- `description?: string`
- `error?: string`
- `required?: boolean`
- `disabled?: boolean`
- `data-testid?: string`

### 규칙

- 에러는 항상 동일 위치에 표시
- label 없으면 aria-label 필수
- field 에러 > form 에러 > toast 순

---

## 2.3 AppSelect / AppCombobox

### Props

- `options: Array<{ value: string; label: string; disabled?: boolean }>`
- `value`
- `onChange`
- `searchable?: boolean`
- `clearable?: boolean`
- `error?: string`
- `loading?: boolean`

### 규칙

- 비동기 검색 시 debounce 기본 적용
- loading 상태 UI 통일

---

## 2.4 AppCheckbox / AppSwitch

### Props

- `checked`
- `onCheckedChange`
- `label?: string`
- `description?: string`
- `disabled?: boolean`

### 규칙

- 즉시 반영 vs 저장 버튼 방식은 팀에서 하나로 통일

---

## 2.5 AppDialog / AppDrawer

### Props

- `open`
- `onOpenChange`
- `title`
- `description?`
- `primaryAction`
- `secondaryAction`
- `danger?: boolean`
- `closeOnOverlayClick?: boolean`

### 필수 요구사항

- focus trap
- ESC 닫기
- scroll lock

---

## 2.6 AppToast / AppAlert

### Toast

- `tone: success | warning | danger | neutral`
- `title`
- `message?`
- `action?`

### Alert (Inline)

- `tone`
- `title`
- `children`
- `dismissible?`

### 규칙

- 서버 에러 → 인라인 우선
- 토스트는 보조적 사용

---

## 2.7 AppTable / AppDataGrid

### Props

- `columns: Array<{ id; header; cell; sortable? }>`
- `rows`
- `loading?`
- `error?`
- `emptyState`
- `pagination?: { page; pageSize; total; onPageChange }`

### 규칙

- loading / error / empty UI는 항상 동일 구조
- sorting/filtering은 URL searchParams 기반 권장

---

# 3. UI Patterns (화면 패턴 규격)

---

## 3.1 Form Layout

구조:

FormHeader (제목/설명)
FormBody (fields)
FormActions (버튼)


### 규칙

- 제출 버튼은 항상 동일 위치
- destructive는 secondary 먼저 배치

---

## 3.2 Loading Pattern

- 버튼 → 내부 로딩
- 리스트/테이블 → skeleton
- 페이지 전환 → route-level loading
- 단순 spinner 단독 사용 최소화

---

## 3.3 Empty State

포함 요소:

- 제목
- 설명
- 다음 행동 CTA

---

## 3.4 Error State

포함 요소:

- 요약 메시지
- 재시도 버튼
- (필요 시) 에러 코드

---

# 4. Layout 규격

---

## 4.1 Container

- 기본 max-width 고정
- 좌우 padding 통일

---

## 4.2 Section Spacing

- 섹션 간 간격은 `space.8` 이상
- Heading 아래 간격은 고정 스케일 사용

---

## 4.3 Z-Index 규칙

- `modal`
- `popover`
- `dropdown`
- `toast`

순서 고정

---

# 5. Accessibility Checklist

- 모든 interactive 요소는 keyboard 접근 가능
- 버튼은 `<button>` 사용 (div 금지)
- label 없는 input은 aria-label 필수
- Dialog는 focus trap 필수
- 색상 대비 WCAG AA 이상

---

# 6. Adapter Layer 구조

src/ui/
tokens/
primitives/ # AppButton, AppInput 등
patterns/ # Form, Table, EmptyState
hooks/ # useToast, useDialog
index.ts


규칙:

- 외부 라이브러리 직접 import 금지
- 라이브러리 교체는 primitives 내부에서만 수정
- Escape hatch 최소화

---

# 7. 운영 규칙

1. UI 라이브러리는 “표현 레이어”
2. Contract는 앱이 소유
3. 패턴은 팀이 강제
4. 새 컴포넌트 추가 시 반드시 규격서 업데이트

---

# 결론

이 문서는 UI 라이브러리와 무관하게:

- 일관성
- 유지보수성
- 교체 가능성
- UX 품질 안정성

을 확보하기 위한 최소 공통 규격이다.

UI 키트는 교체 가능하지만,
UI 규격은 제품의 일부다.
