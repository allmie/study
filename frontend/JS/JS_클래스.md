# 클래스 (JS)
클래스는 동일한 모양의 객체를 더 쉽게 생성하도록 돕는 문법입니다

*객체를 사용해 사용자 데이터 생성*
```typescript
let user1 = {
  name: "js",
  age: 23,
  introduce() {
    console.log(`내 이름은 ${this.name} 입니다`);
  }
}

let user2 = {
  name: "jsw",
  age: 25,
  introduce() {
    console.log(`내 이름은 ${this.name} 입니다`);
  }
}
// ....
```
동일한 프로퍼티를 가지는 객체를 반복해서 생성하게 되고, 중복되는 코드가 나타납니다.

*클래스를 사용해 학생 사용자 생성*
```typescript
class User {
  // 필드
  name;
  age;

  // 생성자
  constructor(name, age){
    this.name = name;
    this.age = age;
  }

  // 메서드
  introduce() {
    console.log(`내 이름은 ${this.name} 입니다`);
  }
}

let user1 = new User("js", 23);
let user2 = new User("jsw", 25);
```
클래스를 사용하면 동일한 형태의 객체를 간결하게 만들 수 있습니다.

클래스 혹은 객체 내에서 `this` 키워드를 사용하면 생성한 객체를 의미합니다. 
`introduce()` 메서드에서 `this.name`은 생성된 user의 이름을 말합니다


## 클래스의 상속

```typescript
class User {
  name;
  age;

  constructor(name, age){
    this.name = name;
    this.age = age;
  }

  introduce() {
    console.log(`내 이름은 ${this.name} 입니다`);
  }
}

class Developer extends User {
  skill;

  constructor(name, age, skill){
    super(name, age)
    this.skill = skill;
  }

  work() {
    console.log(`${this.skill}을 사용합니다`);
  }
}

const developer1 = new Developer("jsw", 30, "js");

developer1.introduce()
developer1.work()
```
`Developer` 클래스는 부모 클래스인 `User` 클래스를 상속합니다. 
`Developer` 클래스는 `User` 클래스의 필드와 메서드를 가지고, 고유의 필드와 메서드를 더 가질 수 있습니다.

`Developer` 클래스에서 고유의 필드만 선언하고, 
생성자에서 `super()` 메서드를 사용해 부모 클래스의 필드를 가져와 설정합니다.
