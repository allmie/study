# 제네릭
타입을 고정된 값으로 선언하지 않고 변수를 사용해 유연하게 사용하는 타입스크립트의 기능입니다. **사용 시점에서 타입을 선언** 합니다

### 제네릭을 사용하지 않는 경우
```typescript
function print(v: any){
 return v;
}

let num = func(10);    // any
let str = func("str"); // any
```
`print()` 함수는 다양한 타입의 매개변수를 그대로 출력합니다. 
매개변수의 타입은 any, unknown 혹은 모든 타입의 union 타입을 사용할 수 있지만 이 때 반환 값의 타입도 동일하게 추론됩니다.

```typescript
function print(v: any){
 return v;
}

let num = print(10);

num.toUpperCase();  // error
num.toFixed();      // error

if(typeof num === "number"){
  num.toFixed();    // ok
}
```
타입이 명확하지 않기 때문에 num 변수는 Number 타입의 메서드와 String 타입의 메서드 둘 다 사용할 수 없습니다.

메서드를 사용하기 위해 타입 가드가 필요합니다.

### 제네릭을 사용하는 경우
```typescript
function print<T>(v: T): T{
 return v;
}

let num = print(10);            // number

console.log(num.toFixed());

let str = print("asdf");        // string

console.log(str.toUpperCase;
```
제네릭을(타입 변수) 사용하면 함수롤 호출할 때 타입을 지정합니다. 숫자 10을 매개변수로 사용하면 number 타입, 문자 "asdf"를 사용하면 string 타입이 됩니다.


## 제네릭 응용
### 원하는 타입 변수를 추론
```typescript
function print<T>(v: T): T{
 return v;
}

let num = print<[number, number, number]>([1, 2, 3]);    // tuple
```
함수 호출 시 타입을 직접 명시할 수 있습니다. 1)코드가 복잡한 경우 타입 추론이 잘못되거나 2)위 코드에서처럼 원하는 타입이 있을 때 타입의 변수를 명시합니다. 
위 코드의 경우 tuple 타입을 명시하지 않았다면 number[] 타입으로 추론 되었을 것입니다.

타입스크립트는 타입 추론 시 더 일반적인(범용적인) 타입으로 추론하기 때문에 원하는 특정 타입이 있다면 직접 명시해야 합니다.

### 타입 변수가 다양한 타입의 배열 추론
```typescript
function print<T>(v: T[]){
  return v;
}

let numArr = print([1, 2, 3]);
let unionArr = print([1, "2", 3]);
```
numArr 변수는 number[] 타입의 매개변수를 전달해서 타입 변수는 number[] 타입으로 추론됩니다.

unionArr 변수는 (string | number)[] 타입의 매개변수를 전달해서 타입 변수는 (string | number)[] 타입으로 추론됩니다.

### 배열에서(tuple) 원하는 값의 타입만 추론
```typescript
function returnFirstValue<T>(data: [T, ...unknown[]]) {
  return data[0];
}

let str = returnFirstValue([1, "hello", "mynameis"]);
```
함수의 매개변수 타입을 정의할 때 배열의 첫번째 요소의 타입을 T, 나머지 요소의 타입은 ...unknown[] 으로 정의합니다.

함수를 호출하면 T는 첫번째 요소의 타입인 number 타입으로 추론됩니다.

### 타입 변수를 제한
타입 변수를 제한할 때 `extends`를 사용합니다.

```typescript
function getLength<T extends { length: number }>(data: T) {
  return data.length;
}

getLength("123");            // ok
getLength([1, 2, 3]);        // ok
getLength({ length: 1 });    // ok

getLength(undefined);        // error
getLength(null);             // error
```
`T extends K` 형태는 T 타입을 K의 서브 타입 혹은 T 타입을 K의 특징으로 제한합니다.

위 코드에서 T 타입은 { length: number }의 서브 타입인 길이를 가지는 타입으로 제한됩니다. undefined와 null 타입은 길이를 가지는 타입이 아니기 때문에 오류가 발생합니다.


## 제네릭 활용
### 제네릭을 사용해서 map 타입 만들기
### 제네릭을 사용해서 foreach 타입 만들기


