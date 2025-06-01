# 인덱스 시그니처
: `{ key: value }` 형태를 가지는 객체가 알 수 없는 여러 개의 key와 value를 가지는 경우 사용한다. 이 때 key와 value는 각각 모두 같은 타입을 사용합니다

프로퍼티 속성에 `string`, `number`, `symbol`, template string pattern 타입만 사용할 수 있습니다
<!-- 미리 유형의 속성의 모든 이름을 알 수 없지만 값의 타입을 알 수 있을 때 사용
string, number, symbol, template string pattern과 이런 타입을 사용한 유니온 타입만 속성으로 사용 가능 -->

인덱스 시그니처 사용
```typescript
type StringArray = {
    [index: string]: string;
}

const strings: StringArray = {
    a: "a",
    b: "b",
    // ...
}
```

## 인덱스 시그니처 충돌
### 예시 1
```typescript
type Strings = {
    [a: number]: string;
    [a: string]: "asdf";
}
```
위 코드에서 오류가 나타납니다
1) number 인덱스는 숫자를 사용한 문자(일부 문자)만 string 인덱스로 취급됩니다.
 `obj[1]`은 `obj["1"]`로 처리됩니다
2) 두 가지 인덱스 모두 동일한 키 공간을 차지합니다.


```typescript
const str1: Strings = {};
str1[1] = "hi"; // ok
console.log(str1["1"]); // error, "asdf" 리터럴 문자열이 아니기 떄문에
```
- number 인덱스는 모든 문자열 사용 가능
- string 인덱스는 리터럴 문자열만 사용 가능하기 때문에 "hi" 문자열 사용 X


**오류 개선**
```typescript
// 1)
type Strings2 = {
    [a: number]: "asdf";
    [a: string]: string;
}

// 2)
type Strings2 = {
    [a: string]: string;
}
```

```typescript
const str2: Strings2 = {};
str2[1] = "asdf"; // ok
console.log(str2["1"]); // ok
```
- number 인덱스는 리터럴 문자열만 가능
- string 인덱스는 모든 문자열 가능


> 더 구체적인 타입(literal)이 덜 구체적인 타입(union 포함)에 할당되는 건 괜찮지만, 반대는 안 됩니다.

### 예시 2
```typescript
type Test = {
    [index: string]: number;
    "aaa": string;
}
```
위 코드에서 오류가 나타납니다.
인덱스 시그니처로 작성한 프로퍼티 key 타입과 동일한 key 타입을 사용하면 value도 같은 타입을 사용하거나 
union 타입을 사용해야 합니다
```typescript
type Test2 = {
    [index: number]: number;
    "aaa": string;
}

type Test3 = {
    [index: string]: number | string;
    "aaa": string;
}
```

참고: [TS 문서](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures)
