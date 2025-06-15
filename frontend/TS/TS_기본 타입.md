# 기본 타입

![TS 기본 타입](https://github.com/user-attachments/assets/c5662ed9-c91e-4118-847b-2b9c1367fe2c)
참고: [한 입 크기로 잘라먹는 타입스크립트](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)


## 원시 타입
: 원시 값을 가지는 타입
  - [원시 값](https://developer.mozilla.org/ko/docs/Glossary/Primitive): Javascript에서, 객체가 아니면서 메서드 또는 속성을(object, function) 가지지 않는 데이터
  - string, number, boolean, null, undefined(, symbol)

원사 값 타입 정의
```typescript
const a: number = 1;
const b: string = "hi";
const c: boolean = true;

c.toUpperCase();  // error
```


## 리터럴 타입
: 하나의 정확한 값을 타입으로 가지는 타입(string, number)

  (type annotation: 타입 주석. 타입을 선언하는 방식 `: number` `: string`...)
```typescript
const literal1: 10 = 10;
const literal2: 20 = 10;  // error

const literal3: "hi" = "hi";
```


## 배열
배열 타입 정의
```typescript
const numberArray: number[] = [1, 2, 3];
const stringArray: string[] = ["a", "b", "c"];
```

제네릭을 사용한 배열 타입 정의
```typescript
const booleanArray: Array<boolean> = [true, false];
```

2개 이상의 타입을 가지는 배열 타입 정의
```typescript
const multiArray: (string | number)[] = ["a", 1];
```

다차원 배열 타입
```typescript
const matrix: number[][] = [[1, 2], [3, 4]];
```

### 튜플
: 길이와 타입이 고정된 값을 가지는 배열의 타입

튜플 타입 정의
```typescript
let tuple: [number, string] = [1, "a"];
tuple = [1, 2, 3];  // error
```

**튜플 사용 시 주의할 점**
- 배열 메서드를 사용하면, 튜플 타입의 배열의 길이가 달라지더라도 문제 없이 사용 가능하다
- 배열 메서드를 사용하더라도 튜플 타입의 정의되지 않은 타입의 값을 사용하면 오류가 나타난다

```typescript
let tuple: [number, number] = [1, 2];
// tuple = ["a", "b"]; // error
// tuple = [4, 5, 7];  // error

tuple.push(3)     // ok, [1, 2, 3]
tuple.pop();
tuple.pop();      // ok, [1]
tuple.push(true); // error
```


## 객체
객체 타입 정의
```typescript
let user: { id: number, name: string } = {
 id: 1, name: "js"
}

// object 타입은 property가 없는 객체입니다.
let user: object = {
 id: 1,
 name: "a"
}
user.id;  // error
```

객체 타입 옵션
선택적 프로퍼티
```typescript
let dog: { name: string, color?: string } = {
 name: "happy"
}
```
읽기 전용 프로퍼티
```typescript
let config: { readonly: string; } = {
 apiKey: "my api key"
}

// config.apiKey = "new api key";  // readonly 타입은 변경 불가능
```


## any
타입스크립트에서만 제공하는 타입 검사를 받지 않는 특수한 타입으로 변수 타입을 확실히 알 수 없을 떄 사용합니다.
모든 타입에서 사용할 수 있습니다.

any 타입을 많이 사용할수록 자바스크립트에 가까워집니다. 필요한 경우에만 사용하는 것이 좋습니다

```typescript
let str: any = "hi";
let num: any = 1;
let bool: any = true;
let arr: any = [1, "a", true];
```


## unknown
any 타입과 비슷하지만 안전한 타입입니다.

unknown 타입은 어떤 타입에도 할당 가능하지만, 
unknown 타입 값은 다른 타입을 입력할 수 없습니다.
```typescript
let a: unknown = "a";
a = 33;
a = () => console.log("unknown");

let num: number = 123;
a = num;
```

```typescript
let num: number;

let unknownVar: unknown;
unknownVar = 1;

num = unknownVar; // error
```


## void
값이 없는 상태를 의미하는 타입. 
반환 값이 없는 함수의 반환 타입, 데이터를 할당하지 않은 변수의 타입을 `void`로 정의합니다.

```typescript
let a: void;

// tsconfig.json > strictNullChecks: false
// a = null;      // error, 할당할 수 없는 값
a = undefined; // void

function voidFunc(): void {
 console.log("no return function");
}
```

## never
불가능한 타입.
함수가 어떠한 값도 반환할 수 없을 때 사용하는 타입입니다.

```typescript
function neverFunction(): never {
 while (true) {}
}
```
함수가 종료되지 않아 반환값이 있을 수 없습니다.

```typescript
function neverFunction2(): never {
 throw new Error();
}
```
오류를 발생하는 함수도 반환값을 never 타입으로 정의할 수 있습니다.

```typescript
let a: never;
a = 1;          // error
a = undefined;  // error
a = () => {};   // error
```
변수에 never 타입을 사용하면 어떤 값도 할당 할 수 없습니다.

### void vs never 변수 선언
두 타입 모두 값을 할당하지 않은 변수를 선언만 할 수 있습니다. 하지만 never 타입으로 선언한 변수는 타입스크립트 옵션에 따라 오류가 나타날 수 있습니다. 흐름상 불가능한 상황인지(never) 의미가 없는 반환인지(void) 파악하는 것이 중요합니다.


## enum
타입스크립트에서만 사용할 수 있는 특별한 타입으로 값에 이름을 지정하고 나열해서 사용합니다. 
타입 선언은 컴파일을 하면 Javascript에서 사라지지만 enum 타입은 객체의 형태로 생성됩니다.

enum 타입에서 나열되는 값은 멤버라고 불립니다.

### 숫자형 enum
```typescript
enum Role {
 ADMIN,
 USER,
 GUEST
}
```
각 멤버에 값을 할당하지 않으면 0부터 순차적으로 1씩 늘어나는 숫자 값이 할당됩니다.

```typescript
enum Role {
 ADMIN, // 0
 USER = 20,
 GUEST  // 21
}
```
기본적으로 멤버는 0부터 자동 할당되며, 멤버에 값이 지정되면 해당 위치부터는 할당된 값을 사용해서 증가합니다.

### 문자형 enum
```typescript
enum Role {
  ADMIN,
  USER,
  GUEST,
}

enum Location {
  seoul = "seoul"
  gyeongsang = "gyeonsangdo",
}

const user = {
  name: "jsw",
  role: Role.ADMIN,        // 0
  location: Location.seoul // "seoul"
};
```
문자형 enum은 모든 값을 반드시 문자로 할당해야 합니다.
