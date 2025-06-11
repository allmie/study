# 제네릭
타입을 고정된 값으로 선언하지 않고 변수를 사용해 유연하게 사용하는 타입스크립트의 기능입니다. **사용 시점에서 타입을 선언** 합니다

## 제네릭 함수
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

console.log(str.toUpperCase());
```
제네릭을(타입 변수) 사용하면 함수롤 호출할 때 타입을 지정합니다. 숫자 10을 매개변수로 사용하면 number 타입, 문자 "asdf"를 사용하면 string 반환값을 타입으로 추론합니다.


## 제네릭 응용
### 타입 변수의 개수
```typescript
function print<T, U, Z>(v1: T, v2: U, v3: Z): [T, U, Z]{
 return [v1, v2, v3];
}

let [a, b, c] = print(123, "asdf", true)
```
타입 변수는 필요한 만큼 사용할 수 있다.

### 원하는 타입 변수를 추론
```typescript
function print<T>(v: T): T{
 return v;
}

let num = print<[number, number, number]>([1, 2, 3]);    // tuple
```
위 코드처럼 원하는 타입이 있다면 타입을 명시합니다.
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

### 배열에서 원하는 값의 타입만 추론
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

위 코드에서 T 타입은 { length: number }의 길이를 가지는 타입으로 제한됩니다. undefined, null 타입은 길이를 가지는 타입이 아니기 때문에 오류가 발생합니다.


### 제네릭 (함수) 활용
- **제네릭을 사용해서 map 타입 만들기**
- **제네릭을 사용해서 foreach 타입 만들기**


## 제네릭 인터페이스
인터페이스에도 타입 변수를 선언할 수 있습니다. 제네릭 인터페이스로 타입을 정의할 때는 반드시 타입을 명시해야 합니다. 

제네릭을 함수의 인자로 사용하면 타입을 추론할 수 있지만, 변수의 타입으로 선언하면 타입을 추론할 수 없기 때문에 타입을 반드시 명시해야 합니다.

### 객체
```typescript
interface User<K, V> {
 id: K;
 name: V;
}

const user1: User<number, string> = {
 id: 1101,
 name: "js"
}

const user2: User<string, string> = {
 id: "asss",
 name: "jsff"
}
```
user의 타입을 User 인터페이스를 사용해 정의합니다.
	
### 인덱스 시그니쳐
```typescript
interface Map<V> {
  [key: string]: V;
}

let stringMap: Map<string> = {
  key: "value",
};

let booleanMap: Map<boolean> = {
  key: true,
};
```
Map 인터페이스는 인덱스 시그니쳐를 사용해 key는 string 타입을, value의 타입은 타입 변수를 가집니다.

stringMap에서 string 타입을 명시해서 value가 string 타입을 가지고</br>
booleanMap에서 boolean 타입을 명시해서 value가 boolean 타입을 가집니다.

### 제네릭 인터페이스 함수
> 참고: [제네릭 인터페이스 함수](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Generic-%ED%83%80%EC%9E%85-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0#%EC%A0%9C%EB%84%A4%EB%A6%AD_%ED%95%A8%EC%88%98_%ED%83%80%EC%9E%85)
(조금은 복잡한 구조)


### 제네릭 인터페이스 활용
```typescript
interface Student {
  type: "student";
  school: string;
}

interface Developer {
  type: "developer";
  skill: string;
}

interface User {
  name: string;
  profile: Student | Developer;
}

function goToSchool(user: User) {
  if (user.profile.type !== "student") {
    console.log("잘 못 오셨습니다");
    return;
  }

  const school = user.profile.school;
  console.log(`${school}로 등교 완료`);
}

const developerUser: User = {
  name: "js",
  profile: {
    type: "developer",
    skill: "typescript",
  },
};

const studentUser: User = {
  name: "철수",
  profile: {
    type: "student",
    school: "서울대",
  },
};

goToSchool(developerUser);  // "잘 못 오셨습니다"
goToSchool(studentUser);    // "서울대로 등교 완료"
```
학생을 의미하는 Student와 개발자를 의미하는 Developer 타입을 정의합니다. 그리고 Student와 Developer 중 하나가 될 수 있는 User 타입을 정의합니다.

Student 타입을 가지는 User만 이용할 수 있는 gotoSchool() 함수를 정의하고, Developer 타입을 가지는 User는 타입 가드를 사용해 분리합니다.

오류 검사 및 실행에는 문제가 없습니다. 하지만 선언한 유저 변수는 타입이 Student 인지 Developer 인지 알 수 없습니다. 그리고 특정 유저를 위한 함수가 생길수록 중복하여 타입 가드를 작성해야 하는 불편함이 있습니다.

*제네릭 인터페이스 사용*
```typescript
interface Student {
  type: "student";
  school: string;
}

interface Developer {
  type: "developer";
  skill: string;
}

interface User<T> {
  name: string;
  profile: T;
}

function goToSchool(user: User<Student>) {
  const school = user.profile.school;
  console.log(`${school}로 등교 완료`);
}

const developerUser: User<Developer> = {
  name: "js",
  profile: {
    type: "developer",
    skill: "typescript",
  },
};

const studentUser: User<Student> = {
  name: "철수",
  profile: {
    type: "student",
    school: "서울대",
  },
};

goToSchool(developerUser);  // error
goToSchool(studentUser);    // "서울대로 등교 완료"
```
제네릭 인터페이스를 사용하면 불필요한 타입 가드를 제거하고, 변수의 타입도 직관적으로 확인할 수 있습니다.

## 제네릭 타입 별칭
```typescript
type Map<V> = {
  [key: string]: V;
}

let stringMap: Map<string> = {
  key: "value",
};

let booleanMap: Map<boolean> = {
  key: true,
};
```
제네릭 인터페이스와 선언하는 방법만 다르고 사용하는 방식은 같습니다.


## 제네릭 클래스
*number 타입의 배열을 생성하는 클래스*
```typescript
class NumberList {
  constructor(private list: number[]) {}

  push(data: number) {
    this.list.push(data);
  }

  pop() {
    return this.list.pop();
  }

  print() {
    console.log(this.list);
  }
}

const numberList = new NumberList([1, 2, 3]);
```
*string 타입의 배열을 생성하는 클래스*
```typescript
class StringList {
  constructor(private list: string[]) {}

  push(data: string) {
    this.list.push(data);
  }

  pop() {
    return this.list.pop();
  }

  print() {
    console.log(this.list);
  }
}

const stringList = new StringList([1, 2, 3]);
```
NumberList, StringList 클래스는 타입만 다를 뿐 배열을 생성하고 사용하는 필드와 클래스 함수는 동일합니다.(비효율)

*제네릭 클래스*
```typescript
class GenericList<T> {
  constructor(private list: T[]) {}

  push(data: T) {
    this.list.push(data);
  }

  pop() {
    return this.list.pop();
  }

  print() {
    console.log(this.list);
  }
}

const genericList1 = new GenericList([1, 2, 3]);
const genericList2 = new GenericList<string>(["a", "b"]);
```
제네릭 클래스는 전달하는 인자에 따라 다른 타입을 추론할 수 있습니다. 인자를 전달하기 때문에 타입 명시를 생략할 수 있습니다.


## 프로미스, 제네릭
타입스크립트에서 Promise는 제네릭 클래스로 구현되어 있습니다. Promise를 만들 때 타입을 명시하면 해당 타입이 resolve 결과 값의 타입이 됩니다.

```typescript
const promise = new Promise<number>((resolve, reject) => {
  setTimeout(() => {
    resolve(20);
    // resolve("asdf");		// error
  }, 3000);
});

promise.then((response) => {
  // response는 number 타입
  console.log(response);
});

promise.catch((error) => {
  if (typeof error === "string") {
    console.log(error);
  }
});
```
비동기 작업인 resolve와 reject 함수의 결과 값은 자동으로 추론할 수 없습니다. 
Promise의 타입 변수를 명시하면 resolve 결과 값의 타입을 설정할 수 있습니다.

모든 경우에서 resolve의 결과 값으로 Promsie의 타입 변수를 정확하게 추론할 수 없기 때문에 **안정성이 저하됩니다**. Promise를 사용하는 경우 타입 변수를 명시해서 타입 안정성을 향상시키는 것이 좋습니다.

Promise의 catch 메서드는 매개변수인 error의 타입을 명확하게 알 수 없습니다. catch 메서드에서 타입 좁히기를 통해 안전하게 사용합니다.

*Promise 타입 변수를 객체로 명시*
```typescript
interface User {
  name: string;
  age: number;
}

new Promise<User>((resolve, reject) => {
  setTimeout(() => {
    resolve({
      name: "jsw",
      age: 30
    });
  }, 3000);
});
```
