# 유틸리티 타입
타입스크립트가 제공하는 특수 타입입니다. 
기존의 제네릭, 조건부 타입, 타입 조작등을 사용하지 않고 타입 변경을 쉽게 사용하도록 돕는 타입입니다.

## Mapped 타입 기반 유틸리티 타입(주요)
### Partial<T>
객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 변환합니다.
```typescript
interface User {
  name: string;
  age: number;
  location: string;
}

const user1: User = {};          // error
const user2: Partial<User> = {}; // ok
```
user1은 User 타입으로 정의해서 모든 프로퍼티가 필수입니다. 빈 객체를 할당하면 오류가 나타납니다.

user2는 `Partial<User>` 타입을 사용해 User 타입의 모든 프로퍼티를 선택적 프로퍼티로 변환합니다. 
빈 객체를 할당해도 정상적으로 동작합니다.

*직접 Partail<T> 구현*
```typescript
type PartialUser<T> = {
  [key in keyof T]?: T[key];
}

const user2: PartialUser<User> = {};    // ok
```

### Required<T>
객체 타입의 모든 프로퍼티를 필수 프로퍼티로 변환합니다.
```typescript
interface User {
  name: string;
  age: number;
  location?: string;
}

const user1: Required<User> = {    // error
  name: "jsw",
  age: 30
}
const user2: Required<User> = {    // ok
  name: "jsw",
  age: 30,
  location: "seoul"
}
```
`Required<User>` 타입을 사용하면 객체의 모든 프로퍼티는 필수 프로퍼티로 변환합니다.

user1은 location 프로퍼티가 생략되었기 때문에 오류가 나타납니다.

*직접 Required<T> 구현*
```typescript
type RequiredUser<T> = {
  [key in keyof T]-?: T[key];
}

const user1: RequiredUser<User> = {};  // error
const user2: RequiredUser<User> = {    // error
  name: "jsw",
  age: 30,
  location: "seoul"
};
```
객체의 타입에서 선택적 프로퍼티를 필수 프로퍼티로 변경할 때 `-?`를 사용한다. **선택적** 기능을 제거한다.

### Readonly<T>
객체의 모든 프로퍼티를 읽기 전용 프로퍼티로 변환합니다. 해당 타입으로 정의된 객체는 프로퍼티의 값을 변경할 수 없습니다.
```typescript
interface User {
  name: string;
  age: number;
  location?: string;
}

const user1: User = {
  name: "jsw",
  age: 30
}
const user2: Readonly<User> = {
  name: "asdf",
  age: 23
}
user1["name"] = "new jsw";  // ok, { name: "new jsw", age: 30 }
user1["age"] = 33;          // ok, { name: "new jsw", age: 33 }
user2["name"] = "fdsa";     // error
user2["age"] = 24;          // error
```
`Readonly<User>` 타입은 모든 프로퍼티를 읽기 전용으로 변환합니다.
`Readonly<User>` 타입으로 정의된 객체의 프로퍼티 값을 수정하면 오류가 나타납니다.

*직접 Readonly<T> 구현*
```typescript
type ReadonlyUser<T> = {
  readonly [key in typeof T]: T[key];
}

const user2: ReadonlyUser<User> = {
  name: "asdf",
  age: 23
}

user2["name"] = "fdsa";    // error
user2["age"] = 24;         // error
```

### Pick<T, K>
객체 타입에서 특정 프로퍼티만 추출하는 타입입니다. T에 객체의 타입을 K에 추출할 프로퍼티 키를 입력합니다.
```typescript
interface User {
  id: number;
  name: string;
  age: number;
  location?: string;
}

const privateUser1: Pick<User, "id"> = {
  id: 3
}
```
`Pick<User, "id">` 타입은 User 객체에서 id 키만 가져와 만들어진 객체 타입입니다.

*직접 Pick<T, K> 구현*
```typescript
type PickUser<T, K extends keyof T> = {
  [key in K]: T[key];
}

const privateUser2: PickUser<User, "id"> = {
  id: 4
}
```
K(프로퍼티 키)의 값은 원시 타입이 아닌 union 타입도 사용할 수 있기 때문에 `PickUser` 타입은 맵드 타입으로 정의합니다.

### Omit<T, K>
객체 타입에서 특정 프로퍼티를 제거하는 타입입니다.
```typescript
interface User {
  id: number;
  name: string;
  age: number;
  location?: string;
}

const privateUser1: Omit<User, "name" | "age" | "location"> = {
  id: 3
}
```
`Omit<User, "name" | "age" | "location">` 타입은 User 객체에서 name, age, location 프로퍼티 키를 제거한 타입입니다.

*직접 Omit<T, K> 구현*
```typescript
type OmitUser<T, K extends keyof T> = {
  [key in keyof T as key extends K ? never : key]: T[key];
}

const privateUser1: OmitUser<User, "name" | "age" | "location"> = {
  id: 3
}
```
1. `key in keyof T` - 객체 T의 프로퍼티 키를 가져옵니다.
2. `as key extends K ? never : key` - 모든 프로퍼티 키 중 K와 겹치는 키를 제외한 키만 사용합니다
3. T 객체의 모든 프로퍼티 키중에 K에 해당하는 프로퍼티를 제거하고 남은 키만 맵드 타입으로 정의합니다

### Record<K, V>
동일한 객체의 값을 가지는 특정 객체의 타입을 정의합니다.
```typescript
interface ImageSize {
  l: { px: number };
  m: { px: number };
  s: { px: number };
}
```
ImageSize 타입은 화면에 따라 다른 버전의 이미지 크기를 정의합니다. 
화면의 크기가 다양해지면 프로퍼티를 추가 하기 때문에 중복 코드가 늘어날 수 있습니다.

*Record<K, V> 사용*
```typescript
type ImageSize = Record<"l" | "m" | "s", { px: number }>

const imageSize: ImageSize = {
  l: { px: 2 },
  m: { px: 1 },
  s: { px: 0 }
}
```

*직접 Record<K, V> 구현*
```typescript
type ImageSize<K extends keyof any, V> = {
  [key in K]: V;
}

type T1 = ImageSize<"l" | "m" | "s", { px: number }>

const imageSize: T1 = {
  l: { px: 2 },
  m: { px: 1 },
  s: { px: 0 }
}
```
- `K extends keyof any` 조건식을 사용하는 이유는 K에 입력되는 키 값이 객체의 키 값으로 사용할 수 있도록 제한(`keyof any`)하기 위함입니다.
- `[key in K]: V` 동일한 프로퍼티 값인 V를 모든 키에 할당하는 맵드 타입을 생성합니다.


## 조건부 타입 기반 유틸리티 타입(주요)
### Exclude<T, U>
특정 타입 T에서 원하는 타입 U를 제거하는 타입입니다. 타입 변수 T가 union 타입이면 조건문을 분산적으로 처리합니다.
```typescript
type T1 = Exclude<string | number, string>;                       // type: number
type T2 = Exclude<string | number | undefined, string | number>;  // type: undefined
```

*직접 Exclude<T, U> 구현*
```typescript
type Exclude_my<T, U> = T extends U ? never : T;
```

### Extract<T, U>
특정 타입 T에서 원하는 타입 U만 추출하는 타입입니다. `Exclude<T, U>` 타입과 마찬가지로 타입 변수 T가 union 타입이면 조건문을 분산적으로 처리합니다.
```typescript
type T1 = Extract<string | number, string>;                       // type: string
type T2 = Extract<string | number | undefined, string | number>;  // type: string | number
```

*직접 Extract<T, U> 구현*
```typescript
ttype Extract_my<T, U> = T extends U ? T : never;
```

### ReturnType<T>
타입 변수 T에 할당된 함수 타입의 반환 값을 추출하는 타입입니다.
```typescript
type T1 = ReturnType<() => string>;   // type: string
type T2 = ReturnType<() => number>;   // type: number
type T3 = ReturnType<() => Promise<number>>;   // type: Promise<number>
```

*직접 ReturnType<T> 구현*
```typescript
type ReturnType_my<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : never;
```
1. `T extends (...args: any) => any` - 타입 변수 T를 함수의 타입으로 제한합니다.(해당 조건식이 없으면 함수가 아닌 타입도 타입 변수로 허용하게 됩니다.)
2. `T extends (...args: any) => infer R ? R : never` - 타입 변수 T(함수)의 반환 값을 추론합니다.

> 참조: [조건부 타입 > infer](/frontend/TS/TS_조건부%20타입.md#infer)
