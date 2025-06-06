# 타입스크립트의 특수 타입

## any
Typescript에서만 제공하는 타입 검사를 받지 않는 특수한 타입으로 변수 타입을 확실히 알 수 없을 떄 사용합니다.
모든 타입에서 사용할 수 있습니다.

`any` 타입을 많이 사용할수록 Javascript에 가까워집니다. 필요한 경우에만 사용하는 것이 좋습니다

```typescript
let str: any = "hi";
let num: any = 1;
let bool: any = true;
let arr: any = [1, "a", true];
```

## unknown
`any` 타입과 비슷하지만 안전한 타입입니다.

`unknown` 타입은 어떤 타입도 사용할 수 있지만, 

`unknown` 타입 값은 다른 타입에서 사용할 수 없습니다
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
반환 값이 없는 함수의 반환 타입, 데이터를 할당하지 않은 변수의 타입을 `void`로 선언합니다

```typescript
let a: void;

// tsconfig.json > strictNullChecks: false
a = undefined; // void
a = null;      // void

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
함수가 종료되지 않아 반환값이 있을 수 없습니다

```typescript
function neverFunction2(): never {
 throw new Error();
}
```
오류를 발생하는 함수도 반환값을 never 타입으로 정의할 수 있습니다

```typescript
let a: never;
a = 1;          // error
a = undefined;  // error
a = () => {};   // error
```
변수에 never 타입을 사용하면 어떤 값도 할당 할 수 없습니다


## enum
Typescript에서만 사용할 수 있는 특별한 타입으로 값에 이름을 지정하고 나열해서 사용합니다. 
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
각 멤버에 아무 것도 할당되지 않으면 0부터 순차적으로 1씩 늘어나는 숫자 값이 할당됩니다.
</br>

```typescript
enum Role {
 ADMIN, // 0
 USER = 20,
 GUEST  // 21
}
```
기본적으로 멤버는 0부터 할당되고, 멤버에 값이 지정되면 해당 위치부터는 할당된 값을 사용해서 증가합니다.

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
