# 클래스 (TS)
> 참고: [JS_클래스](https://github.com/allmie/study/blob/main/frontend/JS/JS_%ED%81%B4%EB%9E%98%EC%8A%A4.md)

타입스크립트에서는 클래스의 필드와 생성자, 메서드에 타입을 정의합니다. 

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

*해결 0*
```typescript
class User {
  // ...
  isAdult(){
    if(this.age! < 19){            // ok
    // ...
  }
}
```
non null 연산자를 사용해 this.age의 타입이 undefined 타입이 아님을 단언합니다.

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
isAdult() 메서드에서 타입 단언을 사용해서 union 타입을 number 타입으로 단언합니다. (복잡한 코드에서는 비추천 방법)

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
해당하는 클래스 메서드에서 조건문을 사용해 필드가 `undefined`인 경우를 처리합니다.

> 타입이 명확해야 한다!

### 클래스 === 타입
클래스는 객체의 타입으로 사용할 수 있습니다.

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
자바스크립트의 클래스와 동일합니다. 추가 필드를 선언하고, 생성자에서 `super()` 메서드를 호출해 부모 클래스의 생성자를 호출합니다.


## 접근제어자
타입스크립트에서만 제공되는 기능으로 클래스의 필드나 메서드의 접근 범위를 설정합니다.
|접근 제어자|접근 가능한 범위|
|---|---|
|`public`|모든 범위|
|`private`|해당 클래스 내부|
|`protected`|해당 클래스와 자식 클래스 내부|

### public
접근 제어자를 지정하지 않으면 기본적으로 사용하고 모든 범위에 접근 가능합니다

```typescript
class User {
  name: string;        // public
  public age: number;

  constructor(name: string, age: number){
    this.name = name;
    this.age = age;
  }

  introduce(){
    console.log(`제 이름은 ${this.name} 입니다.`);
  }
}

const user1 = new User("jsw", 30);

user1.name = "js";
user1.age = 20;
user1.introduce();
```

### private
클래스 내부에서만 접근 가능합니다.

```typescript
class User {
  private name: string;
  public age: number;

  constructor(name: string, age: number){
    this.name = name;
    this.age = age;
  }

  introduce(){
    console.log(`제 이름은 ${this.name} 입니다.`);
  }
}

const user1 = new User("jsw", 30);

user1.name = "js";    // error
user1.age = 20;
user1.introduce();
```
private 으로 지정한 필드는 클래스 외부에서 호출하거나 변경할 수 없습니다. `introduce()` 함수는 클래스 내부에서 해당 필드를 호출하기 때문에 접근 가능합니다.

### protected

```typescript
class User {
  protected name: string;
  private age: number;

  constructor(name: string, age: number){
    this.name = name;
    this.age = age;
  }

  introduce(){
    console.log(`제 이름은 ${this.name} ${this.age}살 입니다.`);
  }
}

class Developer extends User {
  skills: string[];

  constructor(name: string, age: number, skills: string[]){
    super(name, age);
    this.skills = skills;
  }

  work() {
    console.log(`${this.name}은 일합니다.`);
    console.log(`${this.age}인 나이는 문제 없습니다.`);      // error
    console.log(`${this.skills.join(", ")}을 사용합니다`);
  }
}

const user1 = new User("jsw", 30);
const dev1 = new Developer("jsw", 20);

user1.name = "js";    // error
user1.age = 20;       // error
user1.introduce();

dev1.name = "dev1";    // error
dev1.age = 20;       // error
dev1.work();          // ok
```
자식 클래스인 Developer 클래스의 `work()` 메서드에서 User 클래스의 protected 필드인 name은 사용하지만 private 필드인 age는 접근할 수 없습니다. private 필드와 마찬가지로 외부에서 접근할 수 없습니다.

### 필드 생략

```typescript
class User {
  // protected name: string;
  // private age: number;

  constructor(protected name: string, private age: number){
    // this.name = name;
    // this.age = age;
  }

  introduce(){
    console.log(`제 이름은 ${this.name} ${this.age}살 입니다.`);
  }
}
```
생성자의 매개변수에 접근 제어자를 지정하면 동일한 이름의 필드는 선언할 수 없고, 자동으로 필드에 매개변수를 할당합니다.

```typescript
class User {
  constructor(
    protected name: string, 
    private age: number
  ){}

  introduce(){
    console.log(`제 이름은 ${this.name} ${this.age}살 입니다.`);
  }
}
```
타입스크립트에서는 간결하게 클래스를 작성할 수 있습니다.


## 인터페이스 + 클래스
인터페이스를 사용해 클래스의 필드와 메서드를 정의할 수 있습니다.(클래스 설계도)

```typescript
interface UserInterface {
  name: string;
  age: number;
  introduce(): void;
}

class User implements UserInterface{
  constructor(
    public name: string, 
    public age: number,
    protected location: string
    ){}

  introduce(){
    console.log(`제 이름은 ${this.name} ${this.age}살 입니다.`);
  }
}
```
인터페이스를 사용해서 클래스를 정의할 때 인터페이스에서 정의한 필드와 메서드는 클래스에서 **public** 접근 제어자를 사용해야 합니다.

인터페이스로 정의하지 않은 필드나 메서드는 접근 제어자를 자유롭게 사용할 수 있습니다.
