# 함수 타입

## 함수 타입 정의
함수가 어떤 타입의 매개변수를 받고 어떤 타입의 값을 반환하는지를 말합니다.

```typescript
function add(a: number, b:number): number {
 return a + b;
}

const addArrow = (a: number, b: number): number => a + b;
```

### 함수의 반환 타입 추론
```typescript
function add(a: number, b:number) {
 return a + b;
}

const addArrow = (a: number, b: number) => a + b;
```
함수의 반환 타입은 추론 가능하기 때문에 생략 가능합니다.

### 함수의 매개변수 타입 추론
```typescript
function add(a = 0, b = 0) {
 return a + b;
}

const addArrow = (a = 0, b = 0) => a + b;
```
매개변수에 기본 값이 설정되면 매개변수의 타입도 생략 가능합니다.

```typescript
function add(a: string = 0, b = 0) {  // error
 return a + b;
}

add("asdf");  // error
```
기본 값과 매개변수의 타입이 다르거나</br>
기본 값과 다른 타입의 매개변수를 입력하면 오류가 발생합니다.

### 선택적 매개변수
```typescript
function add(a: number = 0, b?:number) {  
 console.log(a);  // 0
 console.log(b);  // undefined, b: number | undefined
}
```
선택적 매개변수를 사용하면 생략이 가능하고 `undefined`와 union 타입으로 추론합니다.

```typescript
function add(a: number = 0, b?:number) {  
 console.log(a);
 if(typeof b === "number"){
  console.log(b);
 }
}
```
매개변수가 union 타입이면 함수를 작성할 때 [타입 좁히기](https://github.com/allmie/study/blob/main/frontend/TS/TS_%ED%83%80%EC%9E%85%20%EC%A2%81%ED%9E%88%EA%B8%B0.md)가 필요합니다.

### 나머지 매개변수
```typescript
function stringSum(...rest: string[]) {  
 return rest.join(" ");
}
```

```typescript
function stringSum(...rest: [string, string]) {  
 return rest.join(" ");
}
```
매개변수의 길이를 제한하려면 튜플 타입을 사용합니다.

## 함수 타입 표현식(function type expression)
함수 타입을 별도로 정의하는 방법.(타입 별칭)

```typescript
type Add = (a: number, b: number) => number

const add: Add = (a, b) => a + b;
```

```typescript
// 함수 타입 표현식을 타입 주석에 직접 사용할 수 있습니다.
const add: (a: number, b: number) => number = (a, b) => a + b;
```

```typescript
type cal = (a: number, b: number) => number

const add: cal = (a, b) => a + b;
const sub: cal = (a, b) => a - b;
const multi: cal = (a, b) => a * b;
const divide: cal = (a, b) => a / b;
```
여러 개의 함수가 동일한 타입을 갖는 경우 유용합니다.

## 호출 시그니쳐(call signature)
함수 타입 표현식과 동일하게 함수의 타입을 별도로 정의하는 방식. Javascript의 객체 형태와 유사합니다.

```typescript
type Add = {
 (a: number, b: number) => number;
}

const add: Add = (a, b) => a + b;
```

```typescript
type Add = {
 (a: number, b: number): number;
 result: number;
}

const add: Add = (a, b) => a + b;
add.result = 0;

add(1, 1);
add.result;
```
호출 시그니쳐는 함수와 프로퍼티를 동시에 정의할 수 있습니다.

## 함수 타입 호환성
특정 함수 타입을 다른 함수 타입으로 정의할 수 있는지 확인합니다.
1. 함수 간 반환 값 타입이 호환 되는지
2. 함수 간 매개변수의 타입이 호환 되는지

### 함수의 반환 값 타입 호환 확인
```typescript
type A = () => number;
type B = () => string;
type C = () => {};

let a: A = () => 100;
let b: B = () => "100";
let c: C = () => 100;

a = b;  // error, 서로소 유니온 타입
a = c;  // ok, 업 캐스팅
b = c;  // error, 서로소 유니온 타입
c = a;  // error, 다운 캐스팅
```
업 캐스팅하는 `a = c`를 제외하고 나머지 함수는 함수가 호환되지 않습니다.

---------------------------
### 함수의 매개변수 값 타입 호환 확인
- 매개변수가 동일한 경우

```typescript
type A = (v: number) => void;
type B = (v: 10) => void;

let a: A = (v) => {};
let b: B = (v) => {};

a = b;  // error
b = a;  // ok 
```
`number 타입` 인 매개변수에 `number literal 타입` 매개변수를 허용할 수 있지만 반대는 불가능합니다.

함수의 매개변수에서는 반환값과는 반대로 다운 캐스팅을 허용하는 것처럼 보여집니다.

```typescript
type Animal = {
  name: string;
};

type Dog = {
  name: string;
  color: string;
};

let animalFunc = (animal: Animal) => {
  console.log(animal.name);
};

let dogFunc = (dog: Dog) => {
  console.log(dog.name);
  console.log(dog.color);
};

animalFunc = dogFunc; // error
dogFunc = animalFunc; // ok
```

`animalFunc = dogFunc`
```typescript
let animalFunc = (animal: Animal) => {
  console.log(animal.name);  // ok
  console.log(animal.color); // error
};
```
`animalFunc` 타입의 매개변수 타입은 `Animal` 타입입니다. `dogFunc` 함수 내부에서 name, color 프로퍼티에 접근하기 때문에 위 코드처럼 함수가 생성됩니다.
`Animal` 객체는 color 프로퍼티 필수 프로퍼티가 아니기 때문에 오류가 나타납니다.

`dogFunc = animalFunc`
```typescript
let animalFunc = (dog: dog) => {
  console.log(animal.name);  
};
```


- 매개변수가 다른 경우

```typescript
type A = (v: number) => void;
type B = (v: 10) => void;

let a: A = (v) => {};
let b: B = (v) => {};

a = b;  // error
b = a;  // ok 
```
