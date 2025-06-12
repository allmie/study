# 조건부 타입
extends 키워드와 삼항 연산자를 이용해 타입을 정의하는 타입스크립트 문법입니다. **조건에 따라 타입을 결정**합니다.

```typescript
type A = number extends string ? number : string;
```
`number extends string` 조건식에서 number 타입은 string 타입으로 제한할 수 없기 때문에 type A는 string 타입이 됩니다.

```typescript
type ObjA = { a: number; }
type ObjB = { a: number; b: number; }

type B = ObjB extends ObjA ? number : string;
```
`ObjB extends ObjA` 조건식에서 ObjB 타입은 ObjA 타입의 서브 타입이기 때문에 조건은 참이 되어 type B는 number 타입이 됩니다.


## 제네릭 조건부 타입
```typescript
type IsString<T> = T extends string ? string : number;

type T1 = IsString<string>;  // type: string
type T2 = IsString<number>;  // type: number

const a: T1 = "string";
const b: T2 = 12345;
```
IsString 타입 변수에 string을 할당하면 string 타입이, number 타입을 할당하면 number 타입이 됩니다.

```typescript
interface isDataString<T extends boolean> {
  data: T extends true ? string : number;
  isString: T;
}

const str: isDataString<true> = {
   data: '홍길동', // String
   isString: true,
};

const num: isDataString<false> = {
   data: 9999, // Number
   isString: false,
};
```
> 예시 출처: https://inpa.tistory.com/entry/TS-📘-타입스크립트-조건부-타입-완벽-이해하기 [Inpa Dev 👨‍💻:티스토리]

isDataString 타입 변수가 true 이면 data는 string 타입, false 이면 data는 number 타입을 가집니다.

*조건부 타입 중첩 사용*
```typescript
type TypeName<T> =
    T extends string ? "string" :
      T extends number ? "number" :
        T extends boolean ? "boolean" :
          T extends undefined ? "undefined" :
            T extends Function ? "function" :
              "object";

type T0 = TypeName<string>;      // "string"
type T1 = TypeName<"a">;         // "string"
type T2 = TypeName<true>;        // "boolean"
type T3 = TypeName<() => void>;  // "function"
type T4 = TypeName<string[]>;    // "object"
```
> 예시 출처: https://inpa.tistory.com/entry/TS-📘-타입스크립트-조건부-타입-완벽-이해하기 [Inpa Dev 👨‍💻:티스토리]

조건부 타입을 중첩해서 사용하면 타입을 유연하게 사용할 수 있지만, 코드가 길어져 가독성이 떨어집니다. 


## 분산적인 조건부 타입
```typescript
type IsString<T> = T extends string ? string : number;

type T1 = IsString<string>;           // type: string
type T2 = IsString<number>;           // type: number
type T3 = IsString<number | string>;  // type: string | number
```
조건부 타입의 타입 변수로 union 타입을 할당하면 분산적인 조건부 타입이 됩니다.

`IsString<number | string>` 타입은 `IsString<number>`, `IsString<string>` 타입으로 각각 분리되어 타입을 검사합니다. 
그리고 분리된 모든 타입은 string, number union 타입이 변환합니다.

```typescript
type T4 = IsString<number | string | boolena>;  // type: number | string
```
T4 타입은 `IsString<number>`, `sString<string>`, `IsString<boolena>` 타입은 number, string, number 타입으로 분리된 후 
union 타입인 number | string 타입이 됩니다.

### 분산되지 않는 조건부 타입
```typescript
type T5 = string | number extends string ? string : number;  // type: number
type T6 = Array<string | number>;                            // type: (string | number)[]
```
T5 타입은 타입 변수없이 타입을 직접 입력합니다.(리터럴 타입) string | number 타입은 하나의 타입으로 인식되어 분산처리 되지 않습니다.

T6 타입은 타입 변수를 사용하지만 `T` 타입 변수를 그대로 사용하지 않고 `T[]` 변환했기 때문에 분산처리 되지 않습니다.


> union 타입을 타입 변수를 사용해서 조건부 타입을 정의하면 분산적으로 처리됩니다.
>
> union 타입을 직접 입력하거나 타입 변수를 `T[]`처럼 변환해서 사용하면 조건부 타입을 분산적으로 처리하지 않습니다.

### 분산, 비분산 조건부 타입 예제
```typescript
type T1 = (1 | 3 | 5 | 7) extends number ? 'yes' : 'no';
type T2<T> = T extends number ? T[] : 'no';              
type T3<T> = T[] extends number ? 'yes' : T[];           

type T4 = T1;                  // type: "yes"
type T5 = T2<(1 | 3 | 5 | 7)>; // type: 1[] | 3[] | 5[] | 7[]
type T6 = T3<(1 | 3 | 5 | 7)>; // type: (1 | 3 | 5 | 7)[]
```
> 예시 출처: https://inpa.tistory.com/entry/TS-📘-타입스크립트-조건부-타입-완벽-이해하기 [Inpa Dev 👨‍💻:티스토리]

T4 타입은 타입 변수를 사용하지 않고 직접 명시했기 때문에 조건 타입문을 분산적으로 처리하지 않습니다.

T5 타입은 타입 변수를 사용하기 때문에 조건 타입문을 분산적으로 처리합니다.

T6 타입은 타입 변수를 변환해서 사용하기 때문에 조건 타입문을 분산적으로 처리하지 않습니다.

### never를 사용하는 조건부 타입
조건부 타입의 결과에 never 타입을 사용하면 해당하는 타입은 제외됩니다.

```typescript
type NeverT<T> = T extends number ? never : T;

type T1 = NeverT<number | string | object>;      // type: string | object
```
1. `number extends number ? never : T` | `string extends number ? never : T` | `object extends number ? never : T`
2. never | string | object
3. string | object

NeverT 타입은 number 타입을 제외한 union 타입이 됩니다.


## 활용
### Extract 타입
```typescript
type Extract_<T, U> = T extends U ? T : never;

type T1 = Extract_< string | number | boolean, string | number>;  // type: string | number
type T2 = Extract_< boolean, string | number>;                    // type: never
```
Extract_ 타입은 T 타입에서 U와 겹치는 타입을 반환합니다.

### Exclude 타입
```typescript
type Exclude_<T, U> = T extends U ? never : T;

type T1 = Exclude_< string | number | boolean, string | number>;  // type: boolean
```
> 예시 출처: https://inpa.tistory.com/entry/TS-📘-타입스크립트-조건부-타입-완벽-이해하기 [Inpa Dev 👨‍💻:티스토리]

Exclude_ 타입은 T 타입에서 U와 겹치는 타입을 제외한 타입을 반환합니다.


## infer
조건부 타입에서 특정 **타입을 추론**하는 타입스크립트의 문법입니다.

### 객체
```typescript
type ValueType<T> = T extends { a: infer U, b: infer U } ? U : never;

type T1 = ValueType<{ a: string, b: string }>;  // string
type T2 = ValueType<{ a: string, b: number }>;  // string | number
```
타입 변수로 입력하는 객체 a, b 프로퍼티 값의 타입을 추론합니다.

### 함수
```typescript
type ReturnType_<T> = T extends () => infer R ? R : never;

type FuncA = () => string;
type FuncB = () => number;
interface FuncC {
   (): number;
}
type D = string;

type A = ReturnType_<FuncA>;  // string
type B = ReturnType_<FuncB>;  // number
type C = ReturnType_<FuncC>;  // number
type D = ReturnType_<D>;      // never
```
함수에서 반환 값의 타입을 추론할 수 있습니다. 추론을 할 수 없다면 거짓으로 처리합니다.

### Promise
```typescript
type PromiseType<T> = T extends Promise<infer R> ? R : never;

type PA = PromiseType<Promise<number>>;  // number
type PB = PromiseType<Promise<string>>;  // string
```
Promise 타입을 추론할 때 타입 변수는 Promise 타입을 사용하고, Promise 타입의 결과를 반환해야 합니다.
