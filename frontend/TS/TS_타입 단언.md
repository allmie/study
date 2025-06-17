# 타입 단언(Type Assertion)
타입스크립트가 타입 추론으로 타입을 판단할 수 없는 경우 추론을 중단하고 값의 타입을 **직접 타입을 명시**하는 것을 말합니다.
- `강제 형변환`과 `타입 단언`은 다릅니다. `형변환`은 자료형을 바꾸지만, `타입 단언`은 타입을 변경하고 실제 데이터와 코드에는 영향을 주지 않습니다

## 타입 단언 사용
```typescript
type Person = {
  name: string;
  age: number;
};

let person1: Person = {};            // error
let person2: Person = {} as Person;  // ok
```
person1 변수에 반드시 있어야 하는 `Person` 객체 프로퍼티가 없기 때문에 오류가 나타납니다.
이 때 `person2` 변수처럼 원하는 타입으로 단언하면 오류를 막을 수 있습니다. `값 as 타입`


```typescript
type Person = {
  name: string;
  age: number;
};

let person = {
 name: "jsw",
 age: 30,
 location: "seoul"
} as Person        // person: Person
```
초과 프로퍼티 검사를 피할 수 있습니다.

하지만 이후 초과 프로퍼티를 사용할 때 문제가 나타날 수 있기 때문에 주의해서 사용해야 합니다.

## 타입 단언 조건
`값 as 타입` 단언식을 `A as B`로 표현해서 사용할 때, `A`와 `B`는 슈퍼-서브 타입 관계를 가져야 합니다.

```typescript
let num1 = 10 as never;    // ok, 업 캐스팅
let num2 = 10 as unknown;  // ok, 다운 캐스팅

let num3 = 10 as string;   // error, 서로소 타입
```
타입을 단언할 때 업 캐스팅과 다운 캐스팅 모두 가능하지만,</br> 관계없는 타입으로의 단언은 할 수 없습니다.

### 다중 단언
```typescript
let num = 10 as unknown as string;
```
다중으로 관계 없는 타입으로도 단언을 할 수 있습니다.</br>
 하지만 오류가 발생할 수 있기 때문에 불필요하고 비효율적인 방법으로 되도록 사용하지 않습니다.

정말 부득이한 경우에만 사용합니다.

### const 단언
```typescript
let num = 10 as const              // num: 10

let numArray = [1, 2, 3] as const  // numArray: readonly [1, 2, 3]
let dog = {                        // dog: { readonly name: "happy"; readonly color: "red";}
 name: "happy",
 color: "red"
} as const
```
`const 단언`을 사용하면 값을 수정할 수 없도록 하는 형태로(리터럴 타입, readonly) 타입을 변경합니다.

### Non null 단언
다른 타입 단언과 다르게 어떤 타입으로 타입을 변경하지 않습니다.
값 뒤에 `!`를 입력해서 해당 값이 `null`이나 `undefined`이 아닌 타입으로 단언합니다.

```typescript
type Dog = {
 name: string;
 age?: number;
}

let dog: Dog = {
 name: "ppoppy",
}

let dogAge: number = dog.age!;  // ok
```
`non null 단언`을 사용하면 age 프로퍼티 타입은 값이 비어있지 않다고 단언합니다.
오류 검사는 우회할 수 있지만 실제 값이 없을 때 사용한다면 코드에서 오류가 나타날 수 있습니다.

> 타입 단언은 단언할 수 있을 때(값이 확실히 있을 때) 사용하는 것을 권장합니다.
