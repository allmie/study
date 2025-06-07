# 인터페이스
타입 별칭처럼 타입의 이름을 정의합니다. 

```typescript
// 타입 별칭
type A =  { a: string; b: number; }

// 인터페이스
interface A {
 a: string;
 b: number;
}
```

```typescript
interface Dog {
 name: string;
 age: number;
 bark: () => void;
}

const dog1: Dog = {
 name: "happy",
 age: 2,
 bark: () => console.log("왈왈")
}

dog1.bark();  // console: "왈왈"
```

### 메서드 오버로딩
매개변수의 개수나 반환값에 따르 다르게 동작하도록 같은 이름의 메서드를 여러개 작성하는 방식
```typescript
interface Dog {
 bark(): void;
 bark(inHouse: boolean): void;
}
```
호출 시그니쳐(call signature)를 사용해서 함수를 정의합니다.

```typescript
interface Dog {
 bark: () => void;
 bark: (inHouse: boolean) => void;  // error
}
```
객체의 메서드를 오버로딩할 때 함수 타입 표현식을 사용하면 식별자가 중복되어 오류가 발생합니다.

*객체 리터럴에서 메서드 오버로딩 사용 시 주의*
```typescript
interface Dog {
    name: string;
    bark(): void;
    bark(inHouse?: boolean): void;
}

const dog: Dog = {
    name: "happy",
    bark(inHouse?: boolean): void {
    if(inHouse === undefined) {
      console.log("no sound")
    }else if(inHouse){
      console.log("쉿")
    }else {
      console.log("wowowowow")
    }
  }
}
```
객체 리터럴에서는 오버로드 시그니쳐를 직접 정의할 수 없습니다. 하나의 메서드에서 타입가드를 사용해서 오버로딩된 메서드를 모두 구현해야 합니다.

### (with Union 타입, Intersection 타입)
```typescript
type Union1 = number | string | { name: string };
type Inter1 = number & string | { name: string };

interface User { name: string } | number;    // error
interface User { name: string } & number;    // error

interface User { name: string }


```
타입 별칭은 `Union 타입` 혹은 `Intersection 타입`을 정의할 수 있지만 인터페이스를 사용하면 정의할 수 없습니다.


*인터페이스 타입을 Union, Intersection 하려면 1*
```typescript
type Union2 = number | string | User;  // ok
type Inter2 = number & string | User;  // ok
```
타입 별칭으로 인터페이스 타입은 `Union 타입` 혹은 `Intersection 타입`으로 사용할 수 있습니다.

*인터페이스 타입을 Union, Intersection 하려면 2*
```typescript
// Union 타입
interface User { name: string }

const user1: User | string = {
 name: "js",
}
const user2: User | string = "empty";
```
```typescript
// Intersection 타입
interface User { name: string }

const user1: User & string = {  // error
 name: "js",
}
const user2: User & string = "empty";  // error
```
`Union 타입`은 타입 주석에 직접 선언할 수 있지만, `Intersection 타입`에서 서로소 유니온 타입을(객체 & 원시 타입) 결합했을 때 변수는 `never 타입`으로 추론되기 때문에 일반적으로 사용하지 않습니다. 


## 인터페이스 확장
다른 인터페이스를 상속 받아 중복되는 프로퍼티를 정의하지 않도록 돕는 타입스크립트의 문법입니다.
```typescript
interface Dog {
    name: string;
    age: number;
    isBark: boolean;
}
interface Cat {
    name: string;
    age: number;
    isScratch: boolean;
}
interface Bird {
    name: string;
    age: number;
    isFly: boolean;
}
```

*인터페이스 확장*
```typescript
interface Animal {
    name: string;
    age: number;
}
interface Dog extends Animal{
    isBark: boolean;
}
interface Cat extends Animal{
    isScratch: boolean;
}
interface Bird extends Animal{
    isFly: boolean;
}
```
중복되는 프로퍼티는 `Animal 타입`으로 분리해서 상속합니다. `Animal 타입`은 `Dog`, `Cat`, `Bird` 타입의 슈퍼 타입이 됩니다.

*다중 확장*
```typescript
interface Dog {
    isBark: boolean;
}
interface Cat {
    isScratch: boolean;
}
interface Monster extends Dog, Cat{}

const monster: Monster = {
    isBark: false,
    isScratch: true,
}
```

*타입 별칭 확장*
```typescript
type Animal = {
    name: string;
    age: number;
}
interface Dog extends Animal{
    isBark: boolean;
}
type Cat extends Animal { ... } // error
```
타입 별칭으로 정의한 객체를 사용해서 인터페이스를 확장할 수 있습니다. 타입 별칭 객체는 확장할 수 없습니다.

### 프로퍼티 재 정의
```typescript
interface Animal {
    name: string;
    age: number;
}
interface MyCat extends Animal{
    name: "happy",
    isScratch: boolean;
}

interface 길고양이 extends Animal{
    name: number,                // error
    isScratch: boolean;
}
```
상속받은 인터페이스의 프로퍼티 타입을 자식 인터페이스에서 재 정의할 수 있습니다. 이 때 타입은 부모 인터페이스 프로퍼티 타입의 서브 타입으로만 사용할 수 있습니다.


## 인터페이스 결합
같은 스코프 내에서 동일한 이름의 인터페이스는 하나의 인터페이스로 사용합니다. 이를 **선언 합침**이라고 말합니다

*타입 별칭 결합 ❌*
```typescript
type Animal = {
    name: string;
}
type Animal = {    // error
    age: number;
}
```
*인터페이스 결합 ✅*
```typescript
interface Animal {
    name: string;
}
interface Animal {
    age: number;
}

const dog: Animal = {
    name: "happy",
    age: 1
}
```

### 프로퍼티 충돌
```typescript
interface Animal {
    name: string;
}
interface Animal {
    name: "happy";      // error
    age: number;
}

const dog: Animal = {
    name: "happy",
    age: 1
}
```
동일한 프로퍼티를 다른 타입으로 정의하면 오류가 나타납니다. 슈퍼-서브 타입 관계, 서로소 유니온 타입등 관계 없이 서로 다른 타입을 사용하면 오류가 나타납니다.
