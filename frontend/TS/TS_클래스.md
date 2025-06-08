# 클래스 (TS)
> 참고: [JS_클래스](https://github.com/allmie/study/blob/main/frontend/JS/JS_%ED%81%B4%EB%9E%98%EC%8A%A4.md)

타입스크립트에서는 클래스의 필드와 생성자에서 타입을 정의합니다. 

### 생성자를 사용하지 않는 클래스
```typescript
class User {
  name: string = "";
  age: number = 19;

  introduce(){
    console.log(`제 이름은 ${this.name} 입니다.`);
  }
}

const user1 = new User();
const user2 = new User();
const user3 = new User();

user1.name = "j";
user2.name = "js";
user3.name = "jsw";

console.log(user1);   // class { name: "j", age: 19 }
console.log(user2);   // class { name: "js", age: 19 }
console.log(user3);   // class { name: "jsw", age: 19 }
```
생성자에서 필드를 초기화하지 않는 경우 필드에 초기값을 설정해야 합니다. 생성자가 없기 때문에 인자를 전달할 수 없고, 객체를 생성하고 프로퍼티에 직접 접근해야 합니다.

### 일반적인 클래스
```typescript
class User {
  name: string;
  age: number;

  constructor(name: string, age: number){
    this.name = name;
    this.age = age;
  }

  introduce(){
    console.log(`제 이름은 ${this.name} 입니다.`);
  }
}
```
생성자에서 필드를 초기화하면 필드의 초기값은 생략할 수 있습니다.


### 선택적 프로퍼티를 사용하는 클래스
선택적 프로퍼티를 사용하면 해당 필드의 타입은 `undefined`와 union 타입이 됩니다.

*생성자에서 선택적 프로퍼티 처리*
```typescript
class User {
  name: string;
  age?: number = 19;

  constructor(name: string, age?: number){
    this.name = name;
    if(typeof age !== "undefined") {
      this.age = age;
    }
  }
}

const user1 = new User("jsw");
const user2 = new User("jsw", 33);

console.log(user1);  // class { name: "jsw", age: 19 }
console.log(user2);  // class { name: "jsw", age: 33 }
```
선택적 프로퍼티를 사용하는 경우 클래스 인자를 생략할 수 있지만, 생략하면 해당 필드는 `undefined` 타입이 입력됩니다.

정상적인 동작을 위해 선택적 프로퍼티 필드가 `undefined`인 경우를 처리합니다.(기본값 사용 혹은 생성자에서 기본값 입력)


*클래스 함수에서 선택적 프로퍼티 처리*
```typescript
class User {
  name: string;
  age?: number = 19;

  constructor(name: string, age?: number){
    this.name = name;
    if(typeof age !== "undefined") {
      this.age = age;
    }
  }

  isAdult(){
    if(this.age < 19){            // error
      console.log("성인이 아닙니다")
    }else {
      console.log("성인입니다")
    }
  }
}

const user1 = new User("jsw");

user1.isAdult();    // console: "성인입니다"
console.log(user1); // class { name: "jsw", age: 19 }
```
클래스 생성과 메서드 실행은 정상적으로 동작하지만, 타입 검사에서 오류가 나타납니다.

`isAdult()` 메서드의 조건문에서 `this.age` 필드는 선택적 프로퍼티 이므로 `undefined`와 union 타입입니다. 
union 타입과 숫자의 비교 연산은 불명확한 비교 연산을 막기 위해 오류로 처리됩니다.

*해결 1*
```typescript
class User {
  name: string;
  age?: number = 19;

  // constructor(...){ ... }

  isAdult(){
    if((this.age as number) < 19){
      console.log("성인이 아닙니다")
    }else {
      console.log("성인입니다")
    }
  }
}

const user1 = new User("jsw");
const user2 = new User("jsw", 3);

user1.isAdult();    // console: "성인입니다"
user2.isAdult();    // console: "성인이 아닙니다"

console.log(user1); // class { name: "jsw", age: 19 }
console.log(user2); // class { name: "jsw", age: 3 }
```
타입 단언을 사용해서 union 타입을 number 타입으로 단언한다. (복잡한 코드에서는 비추천 방법)

*해결 2*
```typescript
class User {
  name: string;
  age?: number = 19;

  // constructor(...){ ... }

  isAdult(){
    if(typeof this.age !== "undefined"){
      if(this.age < 19){
        console.log("성인이 아닙니다")
      }else {
        console.log("성인입니다")
      }
    }
  }
}

const user1 = new User("jsw");
const user2 = new User("jsw", 3);

user1.isAdult();    // console: "성인입니다"
user2.isAdult();    // console: "성인이 아닙니다"

console.log(user1); // class { name: "jsw", age: 19 }
console.log(user2); // class { name: "jsw", age: 3 }
```
해당하는 클래스 함수에서 조건문을 사용해 필드가 `undefined`인 경우를 처리합니다.

> 타입이 명확해야 한다!

### 클래스 === 타입
클래스는 객체의 타입으로서 사용할 수 있습니다.

```typescript
class User {
  name: string;
  age: number;

  constructor(name: string, age: number){
    this.name = name;
    this.age = age;
  }

  introduce(){
    console.log(`제 이름은 ${this.name} 입니다.`);
  }
}

const user1: User = {
  name: "",
  age: 0,
  introduce(){}
}
```


## 클래스의 상속
```typescript
class Developer extends User {
  skills: string[];

  constructor(name: string, age: number, skills: string[]){
    super(name, age);
    this.skills = skills;
  }
}
```
자바스크립트와 동일합니다. 추가 필드를 선언하고, 생성자에서 `super()` 메서드를 호출해 부모 클래스의 생성자를 호출합니다.

## 접근제어자
