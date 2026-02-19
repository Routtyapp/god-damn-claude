---
name: component-usage
description: UI 컴포넌트 사용 가이드. Mantine UI / shadcn/ui 기반으로 언제 어떤 컴포넌트를 써야 하는지 실전 기준을 제공한다.
---

# UI Component Usage Cheat Sheet
> Mantine UI / shadcn/ui 기반 확장 가이드  
> 목적: 컴포넌트 "종류 나열"이 아니라,  
> 언제 무엇을 써야 하는지 빠르게 판단할 수 있는 실전 기준 제공

---

# 1. 빠른 선택 가이드 (Decision Matrix)

## 1.1 사용자 흐름을 막아야 하는가?

- 예 → **Modal / Dialog**
- 아니오 → 아래로

## 1.2 메시지가 일시적인가?

- 예 → **Toast**
- 아니오 → **Inline Alert / Notification Center**

## 1.3 콘텐츠가 병렬 구조인가?

- 예 → **Tabs**
- 아니오 → 아래로

## 1.4 계층 구조인가?

- 예 → **Breadcrumb / Tree**
- 아니오 → 아래로

## 1.5 선택지가 많고 공간이 제한적인가?

- 예 → **Dropdown / Select / Combobox**
- 아니오 → **Radio / Checkbox**

---

# 2. 목적별 컴포넌트 분류

---

# 2.1 Action & Trigger

## Button
### 사용
- 주요 액션 실행
- 폼 제출
- 네비게이션

### 주의
- 한 화면 Primary는 1개 권장
- destructive는 danger 스타일만 사용

---

## IconButton / ActionIcon
### 사용
- 공간이 제한된 UI
- 툴바
- 카드 액션

---

## Toggle / ToggleGroup
### 사용
- 보기 전환 (grid/list)
- 필터 상태

---

# 2.2 Feedback & Status

---

## Toast
### 사용
- 저장 완료
- 복사 완료
- 비차단 성공 안내

### 사용 금지
- 복잡한 에러 설명

---

## Alert (Inline)
### 사용
- 폼 에러
- 중요 공지

---

## Modal / Dialog
### 사용
- 삭제 확인
- 중요한 설정 변경
- 사용자의 명확한 결정 필요

---

## Popover
### 사용
- 버튼 근처 작은 설정
- 필터 옵션

---

## Tooltip
### 사용
- 아이콘 설명
- 보조 정보

---

## Skeleton
### 사용
- 리스트/카드/테이블 로딩
- 구조 유지

---

## Loader / Spinner
### 사용
- 짧은 대기
- 버튼 내부 로딩

---

## Progress / ProgressBar
### 사용
- 업로드
- 다운로드
- 단계 진행

---

# 2.3 Navigation

---

## Tabs
### 사용
- 동일 레벨 콘텐츠 전환
- 병렬 구조

### 사용 금지
- 위계 구조

---

## Breadcrumb
### 사용
- 깊은 페이지 구조
- 관리자 시스템

---

## Tree
### 사용
- 파일 시스템
- 카테고리 계층

---

## Pagination
### 사용
- 대량 데이터
- 테이블/목록

---

## Anchor (Scroll Navigation)
### 사용
- 랜딩 페이지
- 긴 문서

---

## Burger Menu
### 사용
- 모바일 네비게이션
- 사이드바 축소

---

# 2.4 Input & Selection

---

## Input / Textarea
### 사용
- 텍스트 입력

---

## Select / Dropdown
### 사용
- 옵션 많음
- 공간 제한

---

## Combobox
### 사용
- 검색 기반 선택

---

## Radio
### 사용
- 단일 선택
- 옵션 적음 (≤5)

---

## Checkbox
### 사용
- 다중 선택

---

## Switch
### 사용
- 즉시 반영 설정 ON/OFF

---

## DatePicker / TimePicker
### 사용
- 날짜/시간 선택

---

# 2.5 Data Display

---

## Card
### 사용
- 독립 콘텐츠 그룹화

---

## Table / DataGrid
### 사용
- 구조화된 데이터
- 정렬/필터 필요

---

## Badge
### 사용
- 상태 표시
- 태그

---

## Avatar
### 사용
- 사용자 식별

---

## Timeline
### 사용
- 이벤트 흐름 표시

---

## Accordion
### 사용
- FAQ
- 접기/펼치기 정보

---

# 3. SaaS / 마케팅 / 콘텐츠 맥락별 추천

---

## SaaS 대시보드

많이 사용:
- Table / DataGrid
- Breadcrumb
- Tabs
- Modal
- Toast
- Drawer
- Pagination
- Progress

---

## 마케팅 사이트

많이 사용:
- Anchor
- Accordion (FAQ)
- Modal (CTA)
- Toast (문의 완료)
- Carousel
- Tooltip

---

## 콘텐츠 / 커뮤니티

많이 사용:
- Tabs
- Badge
- Toast
- Skeleton
- Infinite Scroll
- Pagination
- Avatar

---

# 4. 잘못된 사용 예

❌ Toast로 중요한 경고 표시  
❌ Modal 남발  
❌ Tabs로 단계형 프로세스 구현  
❌ Dropdown에 3개 옵션만 넣기  
❌ Skeleton 없이 화면 점프 발생  

---

# 5. 컴포넌트 선택 3단계 사고법

1. 사용자의 흐름을 막을 것인가?
2. 메시지가 얼마나 지속되어야 하는가?
3. 정보 구조가 위계인가 병렬인가?

---

# 6. 계층 구조 요약

Action
├─ Button
├─ Toggle
└─ IconButton

Feedback
├─ Toast
├─ Alert
├─ Modal
├─ Popover
└─ Tooltip

Navigation
├─ Tabs
├─ Breadcrumb
├─ Tree
└─ Pagination

Data
├─ Table
├─ Card
├─ Badge
└─ Avatar

Status
├─ Loader
├─ Skeleton
└─ ProgressAction
├─ Button
├─ Toggle
└─ IconButton

Feedback
├─ Toast
├─ Alert
├─ Modal
├─ Popover
└─ Tooltip

Navigation
├─ Tabs
├─ Breadcrumb
├─ Tree
└─ Pagination

Data
├─ Table
├─ Card
├─ Badge
└─ Avatar

Status
├─ Loader
├─ Skeleton
└─ Progress


---

# 7. 결론

컴포넌트는 UI 조각이 아니라  
사용자 인지 흐름을 설계하는 도구다.

적절한 선택은 UX를 단순하게 만들고,  
잘못된 선택은 인터페이스를 복잡하게 만든다.

컴포넌트 선택은 “디자인 취향”이 아니라  
“인지 구조 설계”다.
