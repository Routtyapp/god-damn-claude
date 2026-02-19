# Naming Convention Specification
> 파일명 / 레이어명 / 심볼명 / 에셋명 공통 작명 규칙

목적:
- 개발/디자인 간 충돌 방지
- OS/서버 환경 차이로 인한 오류 예방
- 검색/정렬/관리 효율성 확보
- 일관된 시스템 구축

---

# 1. 기본 원칙

## 1.1 모두 소문자 사용

- 파일명, 폴더명, 레이어명, 심볼명 모두 소문자
- 대문자 사용 금지

이유:
- macOS는 대소문자 구분이 느슨할 수 있음
- Linux 서버는 대소문자 구분
- CI/CD, git 환경에서 충돌 발생 가능

예시:
- icon-user.svg ⭕
- Icon-User.svg ❌

---

## 1.2 첫 글자 숫자 사용 금지

- 파일명 첫 글자에 숫자 사용 금지

이유:
- 일부 빌드 시스템/스크립트 파싱 오류 가능
- 정렬 시 의도치 않은 순서 발생

예시:
- banner-01.png ⭕
- 01-banner.png ❌

---

## 1.3 띄어쓰기 사용 금지

- 공백 사용 금지
- 단어 구분은 `-` 또는 `_` 사용

권장: `-` (하이픈)

이유:
- URL 인코딩 문제 방지
- CLI 환경에서 오류 방지
- 가독성 유지

예시:
- main-banner.png ⭕
- main banner.png ❌

---

# 2. 작명 구조 규칙

## 2.1 기본 구조

유형 + 이름 + 상황


형식:
type-name-state.ext

---

## 2.2 구성 요소 설명

### 1) 유형 (type)
요소의 종류

예:
- icon
- button
- bg
- image
- logo
- thumbnail
- avatar
- illustration
- card
- modal

---

### 2) 이름 (name)
구체적 대상 또는 의미

예:
- favorite
- primary
- mainbanner
- profile
- error
- loading
- header
- footer

---

### 3) 상황 (state)
상태, 변형, 버전

예:
- normal
- hover
- active
- disabled
- selected
- error
- success
- loading
- 01
- 02

---

# 3. 예시

### 아이콘
- icon-favorite-normal.png
- icon-favorite-active.png
- icon-user-disabled.svg

### 버튼
- button-primary-normal.png
- button-primary-hover.png
- button-primary-disabled.png

### 배경
- bg-mainbanner-01.png
- bg-login-02.jpg

### 로고
- logo-main-color.svg
- logo-main-white.svg
- logo-main-symbol.svg

### 썸네일
- thumbnail-product-01.jpg
- thumbnail-user-default.png

---

# 4. 숫자 사용 규칙

## 4.1 숫자는 맨 뒤에서 사용

bg-mainbanner-01.png
image-gallery-02.jpg

## 4.2 두 자리 고정 권장

- 01
- 02
- 03

이유:
- 정렬 시 순서 유지
- 확장성 확보

---

# 5. 레이어명 규칙 (Figma / PSD 등)

## 5.1 컴포넌트 구조

button-primary-normal
card-product-default
modal-login-active


## 5.2 그룹명

- section-hero
- section-feature
- section-footer

## 5.3 반복 요소

- list-item-01
- list-item-02

---

# 6. 폴더 구조 예시

assets/
icon/
bg/
logo/
image/


---

# 7. 금지 사항

- 대문자 사용 금지
- 공백 사용 금지
- 특수문자 사용 금지 (!, @, #, %, &, 등)
- 한글 사용 금지
- 의미 없는 작명 금지 (ex: image1, newfile)

---

# 8. 권장 사항

- 축약어 최소화
- 의미 중심 작명
- 동일 개념은 동일 단어 사용
- 프로젝트 전반 동일 규칙 유지

---

# 9. 요약

- 모두 소문자
- 첫 글자 숫자 금지
- 공백 금지 (`-` 사용 권장)
- 구조: 유형 + 이름 + 상황
- 숫자는 뒤에 두 자리로
- 일관성 유지

---

이 규칙은 디자인과 개발 모두를 위한 공통 언어다.

이름은 단순한 라벨이 아니라,
시스템의 구조다.
