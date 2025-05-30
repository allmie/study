# Typescript 기본 타입

![TS 기본 타입](https://github.com/user-attachments/assets/c5662ed9-c91e-4118-847b-2b9c1367fe2c)
참조: [한 입 크기로 잘라먹는 타입스크립트](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)


## 원시 타입
: 원시 값을 가지는 타입
  - [원시 값](https://developer.mozilla.org/ko/docs/Glossary/Primitive): Javascript에서, 객체가 아니면서 메서드 또는 속성을(object, function) 가지지 않는 데이터
  - string, number, boolean, null, undefined(, symbol)

원사 값 타입 정의
```typescript
const a: number = 1;
const b: string = "hi";
const c: boolean = true;

c.toUpperCase();  // error
```

## 리터럴 타입
: 하나의 정확한 값을 타입으로 가지는 타입(string, number)

  (type annotation: 타입 주석. 타입을 선언하는 방식 `: number` `: string`...)
```typescript
const literal1: 10 = 10;
const literal2: 20 = 10;  // error

const literal3: "hi" = "hi";
```

## 배열
배열 타입 정의
```typescript
const numberArray: number[] = [1, 2, 3];
const stringArray: string[] = ["a", "b", "c"];
```

제네릭을 사용한 배열 타입 정의
```typescript
const booleanArray: Array<boolean> = [true, false];
```

2개 이상의 타입을 가지는 배열 타입 정의
```typescript
const multiArray: (string | number)[] = ["a", 1];
```

다차원 배열 타입
```typescript
const matrix: number[][] = [[1, 2], [3, 4]];
```

### 튜플
: 길이와 타입이 고정된 값을 가지는 배열의 타입

튜플 타입 정의
```typescript
let tuple: [number, string] = [1, "a"];
tuple = [1, 2, 3];  // error
```

**튜플 사용 시 주의할 점**
- 배열 메서드를 사용하면, 튜플 타입의 배열의 길이가 달라지더라도 문제 없이 사용 가능하다
- 배열 메서드를 사용하더라도 튜플 타입의 정의되지 않은 타입의 값을 사용하면 오류가 나타난다

```typescript
let tuple: [number, number] = [1, 2];
// tuple = ["a", "b"]; // error
// tuple = [4, 5, 7];  // error

tuple.push(3)     // ok, [1, 2, 3]
tuple.pop();
tuple.pop();      // ok, [1]
tuple.push(true); // error
```

## 객체
객체 타입 정의
```typescript
let user: { id: number, name: string } = {
 id: 1, name: "js"
}

// object 타입은 property가 없는 객체를 말한다
let user: object = {
 id: 1,
 name: "a"
}
user.id;  // error
```

객체 타입 옵션
선택적 프로퍼티
```typescript
let dog: { name: string, color?: string } = {
 name: "happy"
}
```
읽기 전용 프로퍼티
```typescript
let config: { readonly: string; } = {
 apiKey: "my api key"
}

// config.apiKey = "new api key";  // 변경할 수 없음
```
