# TypeScript 컴포넌트 베스트 프랙티스

## 1) 타입을 먼저 설계하고 구현하기

컴포넌트 코드를 작성하기 전에 아래를 먼저 확정합니다.
- 입력 타입: Props / Options
- 출력 타입: Events / Callback payload
- 상태 타입: 로딩, 에러, 성공 등 상태 전이

```typescript
interface UserCardProps {
  id: string;
  name: string;
  role?: 'admin' | 'editor' | 'viewer';
  onSelect?: (id: string) => void;
}
```

## 2) `any` 대신 `unknown` + 좁히기

```typescript
function parseJson(value: unknown): Record<string, unknown> {
  if (typeof value !== 'object' || value === null) {
    throw new Error('Invalid object');
  }
  return value as Record<string, unknown>;
}
```

## 3) 제어/비제어 패턴을 유니온으로 분리

```typescript
type ControlledProps = {
  value: string;
  onChange: (next: string) => void;
  defaultValue?: never;
};

type UncontrolledProps = {
  defaultValue?: string;
  value?: never;
  onChange?: (next: string) => void;
};

type InputProps = ControlledProps | UncontrolledProps;
```

## 4) 변형(variant)은 문자열 유니온으로 제한

```typescript
type Variant = 'primary' | 'secondary' | 'ghost';
type Size = 'sm' | 'md' | 'lg';

interface ButtonStyleProps {
  variant?: Variant;
  size?: Size;
}
```

## 5) 비동기 상태는 판별 가능한 유니온으로 모델링

```typescript
type AsyncState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: string };

function canRetry<T>(state: AsyncState<T>): boolean {
  return state.status === 'error';
}
```

## 6) 이벤트 payload를 명시적으로 타입화

```typescript
interface SelectEvent {
  id: string;
  source: 'keyboard' | 'mouse';
}

type OnSelect = (event: SelectEvent) => void;
```

## 7) 재사용 가능한 공통 타입 유틸리티 활용

```typescript
type Nullable<T> = T | null;
type WithId<T> = T & { id: string };
type OptionalBy<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>;
```

## 8) 런타임 검증을 타입과 함께 사용

컴파일 타임 타입만으로는 외부 입력(API/사용자 입력)을 보장할 수 없습니다.
- 외부 데이터는 런타임 검증 도구(zod, valibot 등)로 검증
- 검증 성공 데이터만 컴포넌트 상태로 반영

## 9) 네이밍 규칙

- 타입: `ButtonProps`, `LoadState`, `SelectEvent`
- 유니온 값: 소문자 문자열 리터럴 (`'loading'`, `'error'`)
- boolean 이름: `is`, `has`, `can` 접두사 사용

## 10) 최종 점검

- [ ] 공개 API에서 `any` 제거
- [ ] 유니온 분기에서 `never` 누락 없음
- [ ] 기본값과 타입이 일치함
- [ ] 실패 케이스 타입이 성공 케이스와 분리됨
- [ ] 예제 코드가 실제 타입 체커에서 통과함
