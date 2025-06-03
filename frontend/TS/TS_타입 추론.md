# 타입 추론
타입 추론은 타입스크립트에서 타입을 선언하지 않은 변수의 타입을 자동으로 결정하는 것을 말합니다.

## 타입 추론을 하는 경우
### 변수 선언
```typescript
let a = 10;              // a: number
let b = "hi";            // b: string
const c = true;          // c: true

let d = {                // c: { id: number; name: string }
 id: 1,
 name: "jsw"
}
const e = {               // e: { id: number; name: string }
 id: 1,
 name: "jsw"
}
const f = [1, 3, "asdf"]; // f: (string | number)[]
```
변수가 초기화되어 있으면 타입을 추론한다.

`const`를 사용해서 변수를 선언하면 해당 변수는 리터럴 타입으로 추론됩니다.</br>
하지만 원시 값이 아닌 객체나 배열의 경우 `const`를 사용하더라도 리터럴 타입으로 추론하지 않습니다.


### 구조 분해 할당
```typescript
// 배열
let numbers = [1, 2, 3];          // numbers: number[]
let [a, b, c] = numbers;          // a: number; b: number; c: number;
let [e, f, g] = ["a", 33, true];  // e: string; f: number; g: boolean;

// 객체
let { id, name } = c;             // id: number; name: string;
```

### 함수

```typescript
function func0(){}                 // () => void
function func1(params = "test"){}  // (params: string) => void
function func2(){                  // () => string
 return "test 함수"
}  
```
함수에서 매개 변수의 "기본값이 있으면" 매개변수의 타입을 추론합니다. 반환 타입은 return문에 따라 타입을 추론합니다.


## 타입 추론을 못하는 경우
### 변수
```typescript
let d;
d = [1, 2, 3];           // d: any
d.toFixed(2);            // error, 실행하기 전까지 알 수 없음
```

변수를 나중에 할당하더라도 처음 초기화하지 않으면 타입을 추론할 수 없습니다. 타입스크립트의 보호를 받을 수 없습니다.

### 함수
```typescript
function func(params){}  // (params: any) => void
```

함수에서 반환값은 return문이 없어도 타입을 추론할 수 있지만, 매개 변수는 기본값이 없으면 타입을 추론할 수 없습니다.
