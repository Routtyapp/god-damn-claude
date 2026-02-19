# Component Builder Skill

TypeScript 기반 UI 컴포넌트 설계 및 구현 가이드입니다.

## 사용 시점

- 새로운 UI 컴포넌트를 설계하거나 구현할 때
- 기존 컴포넌트 API를 타입 안전하게 리팩터링할 때
- 팀 공통 컴포넌트 규칙을 정리할 때

## 워크플로우

### 1. TypeScript 기준선 확인

프로젝트에서 아래 기준을 먼저 확인합니다.
- `tsconfig.json`에서 `strict: true`
- 공개 API(Props, Events, Return)에 `any` 사용 금지
- 입력 타입과 출력 타입을 먼저 정의한 뒤 구현

### 2. 컴포넌트 API 설계 원칙

#### Props 타입 설계

```typescript
// 좋은 예: 허용 가능한 값을 좁힌 타입
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  onClick?: () => void;
}

// 피해야 할 예: 의미가 넓은 타입
interface ButtonProps {
  variant?: string;
  size?: string;
  state?: any;
}
```

#### 상태 타입 설계

```typescript
type LoadState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; message: string };
```

### 3. 구현 체크리스트

- [ ] 접근성(ARIA, 키보드 내비게이션, 포커스 관리) 반영
- [ ] 제어/비제어 동작을 타입으로 명확히 분리
- [ ] 로딩/에러/빈 상태를 유니온 타입으로 모델링
- [ ] 외부 노출 API에 문서화 가능한 타입 이름 사용
- [ ] 최소 단위 테스트 또는 스토리 예제 작성

## 패턴 문서

상세 패턴은 아래 문서를 참고하세요.

- [TypeScript 컴포넌트 베스트 프랙티스](patterns/typescript.md)
