# 타입 별칭
: 타입을 참조할 수 있는 타입 변수(Type Aliases)

일반적인 객체 타입 정의
```typescript
let user1: {
 id: number;
 name: string;
 nickname: string;
} = {
 id: 1,
 name: "ab",
 nickname: "ss"
}

let user2: {
 id: number;
 name: string;
 nickname: string;
} = {
 id: 2,
 name: "abc",
 nickname: "aa"
}
```

타입 별칭 사용
```typescript
type User = {
 id: number;
 name: string;
 nickname: string;
}

let user1: User = {
 id: 1,
 name: "ab",
 nickname: "ss"
}

let user2: User = {
 id: 2,
 name: "abc",
 nickname: "aa"
}
```
