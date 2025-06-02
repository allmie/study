# 타입 호환
[타입스크립트에서 타입은 집합과 같은 특징을 가집니다.](https://www.inflearn.com/courses/lecture?courseId=330452&type=LECTURE&unitId=156630&subtitleLanguage=ko&tab=curriculum&audioLanguage=ko)
집합은 동일한 속성을 갖는 여러개의 요소들을 하나의 그룹으로 묶은 단위를 말합니다.
</br>

<div align="center">
 <img src="https://github.com/user-attachments/assets/bf599f56-182e-43da-95cd-6b061985a3c5" width="50%" height="50%" title="집합 이미지1" alt="집합1 drawio"></img>
</div>
하나의 타입은 동일한 속성을 가지는 데이터의 집합으로 볼 수 있습니다.</br></br>

<div align="center">
 <img src="https://github.com/user-attachments/assets/c05988b7-607c-4470-9ed4-c9ef114426a7" width="30%" height="30%" title="집합 이미지2" alt="집합2 drawio"></img>
</div>

위의 두 집합은 부모-자식 관계를 가지는 집합입니다.

이처럼 부모-자식 관계를 가지는 타입에서</br>
자식 타입을(서브 타입) 부모 타입 값으로(슈퍼 타입) 취급하는 것을 **업 캐스팅**,</br>
부모 타입을 자식 타입 값으로 취급하는 것을 **다운 캐스팅**이라고 말합니다.

자식 타입은 온전하게 부모 타입에 포함되지만, 부모 타입은 일부만 자식 타입과 공유하기 때문에</br>
**업캐스팅**은 모든 상황에 가능하고, **다운 캐스팅**은 대부분의 상황에서 불가능 합니다.

### 예시
```typescript
let num1: number = 1;  // 슈퍼 타입
let num2: 400 = 400;   // 서브 타입

num1 = num2 // 업 캐스팅 ( 슈퍼 타입 <= 서브 타입 )
num2 = num1 // error, 다운 캐스팅 ( 서브 타입 <= 슈퍼 타입 )
```
</br></br>

## 기본 타입 호환
### 타입 계층도
![타입 계층도](https://github.com/user-attachments/assets/8ce83a32-d782-4004-a584-937e6e9bd31c)

### unknown
<div align="center">
 <img src="https://github.com/user-attachments/assets/48391182-3d92-418c-b426-725f0051ff72" width="35%" height="35%" title="unknown 타입 집합 이미지" alt="unknown 타입 집합 이미지"></img>
</div>

```typescript
let a: unknown;
let b: any;
let c: void;
let d: number = 1;
let e: string[] = ["a", "b", "c"];

a = b;  // ok, unknown <= any
a = c;  // ok, unknown <= void
a = d;  // ok, unknown <= number
a = e;  // ok, unknown <= string[]
```
unknown 타입은 최 상위 타입으로 모든 타입의 값을 업 캐스트 할 수 있습니다.</br></br>

```typescript
let a: unknown;
let b: any;
let c: void;
let d: number = 1;
let e: string[] = ["a", "b", "c"];

b = a;  // ok, any <= unknown
c = a;  // error, void <= unknown
d = a;  // error, number <= unknown
e = a;  // error, string[] <= unknown
```
any를 제외한 어떠한 타입으로도 다운 캐스트 할 수 없습니다.</br></br>


### never
<div align="center">
 <img src="https://github.com/user-attachments/assets/493f265c-cd44-4d71-973f-4efe4ed76ffc" width="50%" height="50%" title="never 타입 집합 이미지" alt="never 타입 집합 이미지"></img>
</div>

never 타입은 최 하위 타입으로 불가능한 타입입니다.</br>변수는 어떠한 값도 할당할 수 없고, 함수도 정상적으로 종료되지 않은 함수에 대해서만 사용 가능하기 때문에 할당할 수 있는 데이터는 없습니다. 이러한 특징은 공집합과 같은데 [공집합은 모든 집합의 부분 집합](https://ko.wikipedia.org/wiki/%EA%B3%B5%EC%A7%91%ED%95%A9)입니다. 


```typescript
let a: unknown
let b: any
let c: void
let d: number = 1
let e: string[] = ["a", "b", "c"]

// 선언된 변수를 초기화 하지 않고 사용하면 에러가 나타나기 때문에, 타입 단언을 사용해 never 타입 값을 임시 할당
let f: never = null as never

a = f;  // ok, unknown <= never
b = f;  // ok, any <= never
c = f;  // ok, void <= never
d = f;  // ok, number <= never
e = f;  // ok, string[] <= never
```
따라서 never 타입은 모든 타입으로 업 캐스트 할 수 있습니다.</br></br>


```typescript
let a: unknown
let b: any
let c: void
let d: number = 1
let e: string[] = ["a", "b", "c"]
let f: never

f = a;  // error, never <= unknown
f = b;  // error, never <= any
f = c;  // error, never <= void
f = d;  // error, never <= number
f = e;  // error, never <= string[]
```
never 타입은 어떤 타입으로도 다운 캐스트 할 수 없습니다.</br></br>


### void
```typescript
let a = (): void => {
   return;
}
let b = (): void => {
   return undefined;    // ok, void <= undefined
}
let c = (): void => {}  // ok, void <= never
```
```typescript
let v: void;

v = undefined; // ok,  void <= undefined

let n: never = null as never;
v = n;         // ok, void <= never
```
void는 undefined와 never 타입의 슈퍼 타입입니다.</br></br>


### any
```typescript
let a: any;
let b: void = undefined;
let c: number = 1;
let d: string[] = ["a"];
let e: object = {};

// 업 캐스팅
a = b;  // ok, any <= void
a = c;  // ok, any <= number
a = d;  // ok, any <= string[]
a = e;  // ok, any <= object

// 다운 캐스팅
b = a;  // ok, void <= any
c = a;  // ok, number <= any
d = a;  // ok, string[] <= any
e = a;  // ok, object <= any
```
any는 예외적으로 모든 타입의 슈퍼 타입이면서 서브 타입입니다. (never 제외)</br></br>


## 객체 타입
```typescript
type Animal = {
  name: string;
  color: string;
};

type Dog = {
  name: string;
  color: string;
  breed: string;
};

let animal: Animal = {
  name: "a",
  color: "green",
};

let dog: Dog = {
  name: "happly",
  color: "red",
  breed: "치와와",
};

animal = dog; // ok, 업 캐스팅
dog = animal; // error, 다운 캐스팅
```
(Animal 타입 - 슈퍼 타입, Dog 타입 - 서브 타입)

Dog 타입이 Animal 타입보다 더 많은 프로퍼티를 가지기 때문에 슈퍼 타입으로 보이지만 그렇지 않습니다.

<div align="center">
 <img src="https://github.com/user-attachments/assets/c104e3c4-56d6-4884-9e4e-21a4b974b412" width="35%" height="35%" title="객체 타입 호환 집합" alt="객체 타입 호환 집합"></img>
</div>

집합으로 생각하면 간단합니다. Animal 타입은 name, color를 가지는 객체이고 Dog 타입은 name, color + breed 프로퍼티를 가지기 때문에 위 그림과 같은 집합이 됩니다.

따라서 Dog 타입에 포함되는 객체는 Animal 타입이 될 수 있지만 그 반대는 허용하지 않습니다. </br>

### 초과 프로퍼티 검사
변수를 **객체 리터럴로 초기화**할 때, 타입에 정의된 프로퍼티 외의 초과된 프로퍼티를 갖는 객체를 변수에 할당 할 수 없도록 막는 검사입니다.(객체 리터럴로 초괴화하면 해당 객체가 어떤 타입의 서브 타입인지 알 수 없다)

```typescript
type Animal = {
  name: string;
  color: string;
};

type Dog = {
  name: string;
  color: string;
  breed: string;
};

// 객체 리터럴
let dog0: Animal = {
   name: "harry",
   color: "brown",
   breed: "bulldog"    // error, 초과 프로퍼티 검사
}

let dog1: Animal;
let dog2: Dog = {
   name: "happy",
   color: "red",
   breed: "bulldog"
}

dog1 = dog2;  // ok, 업 캐스팅
```
