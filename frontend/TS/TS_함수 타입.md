# 함수 타입

## 함수 타입 정의
함수가 어떤 타입의 매개변수를 받고 어떤 타입의 값을 반환하는지를 말합니다.

```typescript
// 함수 표현식
function add(a: number, b:number): number {
 return a + b;
}

// 화살표 함수
const addArrow = (a: number, b: number): number => a + b;
```

### 함수의 반환 타입 추론
```typescript
// 함수 표현식
function add(a: number, b:number) {
 return a + b;
}

// 화살표 함수
const addArrow = (a: number, b: number) => a + b;
```
함수의 반환 타입은 추론 가능하기 때문에 생략할 수 있습니다.

### 함수의 매개변수 타입 추론
```typescript
// 함수 표현식
function add(a = 0, b = 0) {
 return a + b;
}

// 화살표 함수
const addArrow = (a = 0, b = 0) => a + b;
```
매개변수에 기본 값이 설정되면 매개변수의 타입을 생략할 수 있습니다.

```typescript
// 함수 표현식
function add(a: string = 0, b = 0) {  // error
 return a + b;
}

// 화살표 함수
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
선택적 매개변수를 사용하면 해당 값은 생략할 수 있고, `undefined`와 union 타입으로 추론합니다.

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

### 화살표 함수 사용
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

### 함수 표현식 사용
```typescript
type Add = (a: number, b: number) => number

let add: Add = function (a, b){
  return a + b;
}
```

## 호출 시그니쳐(call signature)
함수 타입 표현식과 동일하게 함수의 타입을 별도로 정의하는 방식. Javascript의 객체 형태와 유사합니다.

```typescript
type Add = {
 (a: number, b: number): number;
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
호출 시그니쳐는 프로퍼티와 동시에 정의할 수 있습니다.

## 함수 타입 호환성
특정 함수 타입을 다른 함수 타입으로 정의할 수 있는지 확인합니다.
1. 함수 간 **반환 값** 타입이 호환 되는지
2. 함수 간 **매개변수** 의 타입이 호환 되는지
   - 할당하려는 함수의 매개변수 개수가 같거나 더 적어야 합니다
   - 대응하는 매개변수가 할당 가능해야 합니다

### 함수의 반환 값 타입 호환 확인
```typescript
type A = () => number;
type B = () => string;
type C = () => 100;

let a: A = () => 10;
let b: B = () => "100";
let c: C = () => 100;

a = b;  // error, 서로소 유니온 타입
a = c;  // ok, 업 캐스팅
b = c;  // error, 서로소 유니온 타입
c = a;  // error, 다운 캐스팅

console.log(a);   // console: 10
```
`a = c`는 반환 값의 타입이 업 캐스팅되어 할당 가능합니다. (호환 가능)

함수를 대체하면 함수 구현부는 변경됩니다.



### 함수의 매개변수 값 타입 호환 확인
매개변수의 타입은 더 일반적인 타입으로 대체(타입 넓히기)는 허용하지만</br>
더 구체적인 타입으로 대체(타입 좁히기)는 안전하지 않아 허용하지 않습니다

함수의 매개변수에서는 반환값과는 반대로 다운 캐스팅을 허용하는 것처럼 보여집니다.

- 매개변수가 동일한 경우
*매개 변수가 숫자와 숫자 리터럴인 함수*
```typescript
type A = (v: number) => void;
type B = (v: 10) => void;

let a: A = (v) => {
 console.log('a 함수');
};
let b: B = (v) => {
 console.log('b 함수');
};

// a = b;  // error
b = a;     // ok
```
`a 함수`는 모든 숫자를 허용, `b 함수`는 `10` 만 허용합니다.</br>
`a(5)` 를 실행하면 `b(5)`가 호출되는데, 이 때 `b 함수`는 숫자 5를 허용하지 않습니다.(`a = b`)

`b(10)` 을 실행하면 `a(10)`가 호출되는데, 이 때 `a 함수`는 숫자 10을 허용합니다.(`b = a`)

 - a 함수는 매개변수가 number인 함수, b 함수는 매개변수가 숫자 10만 허용하는 함수!

```typescript
b(10);     // console: 'a 함수', b: (v: 10) => void
// b(100); // error
```
변수(함수)를 선언하면 타입은 **고정** 됩니다. 매개변수의 타입이 `10`인 `type B`는 유지됩니다. 

하지만 매개변수의 타입은 호환되기 때문에 값(함수의 구현부)는 대체 됩니다.

*매개 변수가 객체인 함수*
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
animalFunc 함수는 Animal 타입과 서브 타입인 Dog 타입을 매개변수로 허용합니다.
dogFunc 함수는 Dog 타입을 매개변수로 허용합니다.(매개변수가 객체인 경우는 일반적인 경우와 다르게 다운캐스팅을 허용합니다)

**animalFunc = dogFunc**
animalFunc([Animal type])을 실행하면 dogFunc([Animal type])을 호출하는데,
dogFunc 함수는 Animal 타입을 허용하지 않습니다.

**dogFunc = animalFunc**
dogFunc([Dog type])을 실행하면 animalFunc([Dog type])를 호출하는데,
animalFunc 함수는 Animal 타입의 서브 타입인 Dog 타입을 허용합니다.

| animalFunc 매개 변수 | 허용 |
| --- |:---:|
| Animal type | ✅ |
| Dog type | ✅ |

| dogFunc 매개 변수 | 허용 |
| --- |:---:|
| Animal type | ❌ |
| Dog type | ✅ |

*함수 대체*
```typescript
// animalFunc = dogFunc
animalFunc = (animal: Animal) => {
  console.log(animal.name);  // ok
  console.log(animal.color); // error
};

// dogFunc = animalFunc
dogFunc = (dog: Dog) => {
  console.log(animal.name);  // ok
};
```

- 매개변수가 다른 경우
```typescript
type A = (v: number) => void;
type B = (v1: number, v2: string) => void;
type C = (v1: string, v2: string) => void;

let a: A = (v) => {};
let b: B = (v1, v2) => {};
let c: C = (v1, v2) => {};

a = b;  // error
a = c;  // error
b = a;  // ok
```
매개변수의 개수가 적은 함수에서 많은 함수로 할당할 수 있습니다.
대응하는 매개변수의 타입이 호환되지 않으면 함수는 할당할 수 없습니다.(`a = c`)

> *매개변수가 다른 경우는 매개변수가 객체인 경우로 생각해서 비교하면 간단합니다. 매개변수가 많은 경우가 프로퍼티가 더 많은 객체(서브 타입), 적은 경우가 프로퍼티가 더 적은 객체(슈퍼 타입)


## 함수 오버로딩
함수를 매개변수의 개수나 타입에 따라 다르게 동작하도록 만드는 방식입니다.
- 오버로드 시그니처: 구현부 없이 선언부만 생성한 여러가지 버전의 함수
```typescript
// 오버로드 시그니쳐
function func(a: number): void;
function func(a: number, b: number): void;

// 실제 구현부 -> 구현 시그니쳐
function func(a: number, b?: number){
 if(typeof b === "number"){
  console.log(a + b);
 }else {
  console.log(a);
 }
}

func(1);
func(3, 5);
```
구현 시그니쳐의 매개변수는 모든 오버로드 시그니쳐와 호환해야 합니다. 달라지는 매개변수에 맞게 타입가드를 작성합니다.
