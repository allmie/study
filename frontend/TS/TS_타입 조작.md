# 타입 조작
타입 조작은 타입스크립트는 타입을 유동적으로 다른 타입으로 변환하는 기능입니다.

제네릭, 인터페이스, 타입 별칭도 타입 조작의 일부입니다.

> 타입을 정의할 때 **interface**를 주로 사용
>
> 타입을 조작해서 재정의 할 때는 **타입 별칭**을 사용

## [제네릭](https://github.com/allmie/study/blob/main/frontend/TS/TS_%EC%A0%9C%EB%84%A4%EB%A6%AD.md)

## 인덱스드 엑세스 타입
객체, 배열, 튜플 타입에서 특정 프로퍼티나 요소만 추출해서 정의하는 타입입니다. 인덱스를 이용해 프로퍼티의 타입을 가져옵니다.

```typescript
interface Post {
  id: number;
  contents: string;
  author: {
   id: string;
   name: string;
   age: number;
  }
}

function getUserInfo(user: { id: string, name: string, age: number }){
  return user;
}
```
getUserInfo 함수에서 매개변수인 user 객체의 프로퍼티인 id, name, age 의 타입을 각각 정의한니다.

```typescript
function getUserInfo(user: Post["author"]){
  return user;
}
```
인덱스트 엑세스 타입을 사용해 Post 인터페이스의 author 프로퍼티를 추출해서 매개변수 user의 타입을 정의한니다. 
user의 프로퍼티가 수정되더라도 Post 인터페이스로 간단하게 처리할 수 있습니다.

```typescript
const key = "author";

function getUserInfo(user: Post[key]){  // error
  return user;
}
```
인덱스에 변수를 사용할 수 없습니다.

`Post["author"]["name"]` 인덱스를 중첩해서 사용할 수 있습니다.

*튜플 타입 추출*
```typescript
type Tup = [number, number, string];

type Tup0 = Tup[0];      // number
type Tup1 = Tup[1];      // number
type Tup2 = Tup[2];      // string
type Tup3 = Tup[number]; // number | string
```
인덱스드 엑세스 타입을 사용해 각 요소의 타입을 추출할 수 있습니다.

## keyof 연산자
객체 타입에서 타입 내 프로퍼티의 키를 문자 리터럴 union 타입으로 추출하는 연산자 입니다.

```typescript
interface User {
  name: string;
  age: number;
}

function getUserValue(user: User, key: "name" | "age"){
  return user[key];
}

const user: User = {
  name: "jsw",
  age: 30,
}

getUserValue(user, "name");  // "jsw"
getUserValue(user, "age");   // 30
```
getUserValue 함수는 객체와 프로퍼티 key를 매개변수로 받아 해당하는 value를 반환합니다. 
위 코드처럼 key 값을 union 타입으로 명시 하면 오타 혹은 객체의 프로퍼티가 변화할 때 오류가 나타날 수 있기 때문에 관리의 번거로움이 있습니다.

```typescript
interface User {
  name: string;
  age: number;
}

function getUserValue(user: User, key: keyof User){
  return user[key];
}

const user: User = {
  name: "jsw",
  age: 30,
}

getUserValue(user, "name");  // "jsw"
getUserValue(user, "age");   // 30
```
keyof 연산자를 사용해서 User 타입의 모든 프로퍼티 키를 문자 리터럴 union 타입으로 가져옵니다.(keyof User = "name" | "string")

keyof 연산자는 뒤에는 값이 아닌 **타입**만 사용 가능합니다.(`keyof [타입]`)

### with typeof
typeof 연산자는 특정 값의 타입을 문자열로 반환합니다. 타입을 정의할 때 사용하면 특정 변수의 타입을 추론할 수 있습니다.

```typescript
const num1 = 123;
let num2 = 444;
const str = "asdf";
const user1 = {
  name: "jsw",
  age: 30,
}

type Num1 = typeof num1;  // type: 123
type Num2 = typeof num2;  // type: number
type Str = typeof str;    // type: "asdf"
type User = typeof user1; // type: { name: string; age: number }

const user2: User = {
  name: "js",
  age: 3
}
```
typeof 연산자를 사용해서 타입을 정의하면 변수에 따라 특정 타입이나 리터럴 타입으로 추론합니다.

```typescript
interface User {
  name: string;
  age: number;
}

const user: User = {
  name: "jsw",
  age: 30,
}

function getUserValue(user: User, key: keyof typeof user){
  return user[key];
}
```
keyof와 typeof 연산자를 함께 사용하면 타입이 아닌 **값**을 사용해 타입을 정의할 수 있습니다.


## Mapped 타입
객체 타입을 기반으로 새로운 객체 타입을 만드는 타입입니다.

user1 객체와 타입, user1의 정보를 수정하는 함수를 정의합니다.
```typescript
interface User {
  id: number;
  name: string;
  age: number;
}

const user1 = {
  id: 1,
  name: "jsw",
  age: 30
}

function updateUser(updateUser1: User){
  for(let key in updateUser1){
    user1[key] = updateUser1[key];
  }
}

updateUser({ id: 33 });  // error
updateUser({ id: 33, name: "new name", age: 30 });  // ok
```
updateUser의 매개변수의 타입을 User 타입으로 사용하면 수정 시 수정을 원하지 않는 User의 프로퍼티도 그대로 작성해야 합니다. 
수정을 원하지 않더라도 값을 잘못 입력하면 수정됩니다.

```typescript
interface UpdateUser1 {
  id?: number;
  name?: string;
  age?: number;
}

function updateUser(updateUser1: User){
  for(let key in updateUser1){
    user1[key] = updateUser1[key];
  }
}

updateUser({ id: 33 });  // ok
```
별도의 UpdateUser 타입을 정의해 User의 모든 값을 선택적 프로퍼티로 변경하면 수정을 원하는 프로퍼티만 입력해서 user를 수정할 수 있습니다.

하지만 User와 UpdateUser 타입은 중복되는 프로퍼티를 정의합니다. 이 때 맵드 타입을 사용합니다.
```typescript
type NewUpdateUser1 = {
  [key in keyof User]?: User[key];
}

function updateUser(updateUser1: NewUpdateUser){
  for(let key in updateUser1){
    user1[key] = updateUser1[key];
  }
}

updateUser({ id: 33 });  // ok
```
맵드 타입을 사용하면(+ keyof) 중복되는 기존 객체 타입을 한 줄로 변환할 수 있습니다.

맵드 타입은 인터페이스로 정의할 수 없습니다. 타입 별칭을 사용합니다.


## 템플릿 리터럴 타입
문자 리터럴 타입을 기반으로 정해진 패턴의 문자열만 포함하는 타입입니다.

```typescript
type Color = "red" | "black" | "green";
type Animal = "dog" | "cat" | "chicken";

type ColoredAnimal = `red-dog` | 'red-cat' | 'red-chicken' | 'black-dog' ... ;
```
ColoredAnimal 타입은 Color와 Animal 타입을 조합한 문자 리터럴 타입입니다.

*템플릿 리터럴 타입*
```typescript
type ColoredAnimal = `${Color}-${Animal}`;
```


## [조건부 타입](https://github.com/allmie/study/blob/main/frontend/TS/TS_%EC%A1%B0%EA%B1%B4%EB%B6%80%20%ED%83%80%EC%9E%85.md)
