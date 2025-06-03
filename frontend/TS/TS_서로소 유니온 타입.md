# 서로소 유니온 타입
교집합이 없는 타입 여러 개를 가지는 `Union 타입`을 말합니다

## 간단한 서로소 유니온 타입
```typescript
type suroso = number | string | boolean;
```

## 복잡한 서로소 유니온 타입(간단한 프로그램)
```typescript
type Admin = {
  name: string;
  kickCount: number;
};

type Member = {
  name: string;
  point: number;
};

type Guest = {
  name: string;
  visitCount: number;
};

type User = Admin | Member | Guest;

function login(user: User) {
  if ("kickCount" in user) {
		// Admin
    console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
  } else if ("point" in user) {
		// Member
    console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
  } else {
		// Guest
    console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
  }
}
```
역할에 따라 타입이 다르고, 함수를 실행했을 때 동작하는 메서드가 다릅니다.

매개 변수 객체의 프로퍼티를 통해 역할을 구분하는데, 타입 가드가 어떤 타입을 구분하는지 직관적이지 않습니다.

이런 경우에 구분할 수 있는 프로퍼티를 추가 정의하면 좋습니다.
```typescript
type Admin = {
  tag: "ADMIN";
  name: string;
  kickCount: number;
};

type Member = {
  tag: "MEMBER";
  name: string;
  point: number;
};

type Guest = {
  tag: "GUEST";
  name: string;
  visitCount: number;
};

type User = Admin | Member | Guest;

function login(user: User) {
  if (user.tag === "ADMIN") {
    console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
  } else if (user.tag === "MEMBER") {
    console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
  } else {
    console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
  }
}
```

### +enum 타입
```typescript
enum Role {
 ADMIN = "ADMIN",
 MEMBER = "MEMBER",
 GUEST = "GUEST"
}

type Admin = {
  tag: Role.ADMIN;
  name: string;
  kickCount: number;
};

type Member = {
  tag: Role.MEMBER;
  name: string;
  point: number;
};

type Guest = {
  tag: Role.GUEST;
  name: string;
  visitCount: number;
};

type User = Admin | Member | Guest;

function login(user: User) {
  if (user.tag === Role.ADMIN) {
    console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
  } else if (user.tag === Role.MEMBER) {
    console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
  } else {
    console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
  }
}
```
