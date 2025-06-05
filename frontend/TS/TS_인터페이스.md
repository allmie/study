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

*객체 리터럴*
```typescript
interface Dog {
 bark(): void;
 bark(inHouse?: boolean): void;
}

const dog: Dog = {
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

type Inter2 = number & string | { name: string };

interface User { name: string } | number;    // error
interface User { name: string } & number;    // error
```
인터페이스를 사용해서 Union 타입이나 Intersection 타입을 정의할 수 없습니다.


## 인터페이스 확장
