# 타입 좁히기

매개 변수가 `Union 타입`인 어떤 함수가 있습니다.
```typescript
function func(value: number | string) {
 value.toUpperCase();   // error
 value.toFixed(2);      // error
}
```
이 함수는 매개 변수가 `number 타입`일 때와 `string 타입`일 때 실행할 메서드가 동시에 있습니다.
하지만 하나의 타입의 매개 변수는 2가지 메서드를 모두 실행할 수 없기 때문에 오류가 나타납니다

이 오류를 해결하려면 조건문을 사용해서 `number 타입`일 때와 `string 타입`일 때를 구분해서 메서드를 사용해야 합니다
```typescript
function func(value: number | string) {
 if(typeof value === "string"){
  value.toUpperCase();
 }
 if(typeof value === "number"){
  value.toFixed(2);
 }
}
```
이렇게 조건문을 사용하면 조건문 내부에서는 타입이 구분됩니다.
매개 변수가 처음 조건문에서는 `string 타입`, 2번째 조건문에서는 `number 타입`이 됩니다. 이러한 방식을 **타입 좁히기**라고 말합니다.

> 조건문과 `typeof`를 사용해 타입을 좁히는 표현식을 ***타입 가드*** 라고 말합니다.

## 타입 가드
타입스크립트에서 오류를 줄일 수 있는 코드 작성 방법입니다.

### 일반 타입 가드
일반적인 변수의 타입을 좁히는 타입 가드의 키워드

```typescript
function func(value: number | string) {
 if(typeof value === "string"){
  value.toUpperCase();
 }
 if(typeof value === "number"){
  value.toFixed(2);
 }
}
```

### 클래스 타입 가드
내장 클래스 타입을 보장할 수 있는 타입 가드

```typescript
function func(value: number | string | Date) {
 if(value instanceof Date){
  value.getDate();
 }
}
```
value가 Date 클래스인지 구분하는 타입 가드

### 배열 타입 가드
배열을 보장하는 타입 가드의 키워드

```typescript
function func(param: string | string[]){
 if(Array.isArray(param)){
  param.join(" ");
 }
 if(typeof param === "string"){}
}
```

### 객체 타입 가드
주로 객체 속성을 보장하는 타입 가드의 키워드

```typescript
type Dog = {
 name: string;
 age: number;
 isBark: boolean;
}

type Cat = {
 name: string;
 age: number;
 isScratch: boolean;
}

function func(value: Cat | Dog) {
 if(value && "isBark" in value){
  console.log(value.name);    // Dog type
 }
 if(value && "isScratch" in value){
  console.log(value.name);    // Cat type
 }
}
```
value가 age 프로퍼티를 가지는 객체 타입일 때를 구분하는 타입 가드

객체의 속성을 통해 객체의 타입을 구분하지만, 정확하게 특정 타입을 찾는 것은 아니라고 생각합니다.
그래서 고유한 속성을 파악하는 것이 중요합니다

(### 사용자 정의 타입 가드)
(### interface 타입 가드)
