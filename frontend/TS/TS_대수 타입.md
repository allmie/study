# 대수 타입
대수 타입은 여러 개의 타입을 합성해서 만드는 타입을 말합니다. 합집합 타입과(Union 타입) 교집합 타입이(Intersection 타입) 있습니다.

## Union 타입
합집합. Union 타입은 두 개 이상의 타입을 허용하는 경우를 말합니다. 허용할 수 있는 타입이나 타입의 개수에 제한이 없습니다.

```typescript
let a: string | number;

a = 1;
a = "aa";

type Func = () => void;

let b: string | number | boolean | Func;

b = 1;
b = "asdf"
b = () => console.log("hi");
```

### Union 배열 타입
```typescript
let a: (string | number)[];

a = ["a", 123];
```
### Union 객체 타입
```typescript
type Dog = {
  name: string;
  color: string;
};

type Person = {
  name: string;
  language: string;
};

type Union1 = Dog | Person;

let a: Union1;

let a: Union1 = {
   name: "d",
   color: "red",
   // language: "ko"
}
let b: Union1 = {
   // color: "red",
   name: "dd",
   language: "ko"
}
let c: Union1 = {
   language: "ko"
}    // error
let d: Union1 = {
   name: "d",
}    // error
```
<div align="center">
 <img src="https://github.com/user-attachments/assets/43d820b1-575e-499b-abba-f7766b9c329e" width="50%" height="50%" title="Union 타입 집합" alt="Union 타입 집합 drawio"></img>
</div>

위 그림에서 B를 제외한 A와 B타입은 `Dog` 타입과 `Person` 타입 어디에도 속하지 않는다.

그렇기 때문에 c와 d 객체의 경우 `Dog` 혹은 `Person` 객체 타입 어디에도 속하지 않기 때문에 오류가 나타납니다. 


## Intersection 타입
교집합. 2개 이상의 타입을 조합하는 경우를 말합니다.(and)

```typescript
let n: never = null as never;
let v: void;

let variable: string & number;

variable = n;
variable = v;  // error
```

<div align="center">
 <img src="https://github.com/user-attachments/assets/a48178aa-ba57-4eac-a4d1-f5612db0dcfa" width="50%" height="50%" title="Union 타입 집합" alt="Union 타입 집합 drawio"></img>
</div>

두 집합은 교집합이 없기 때문에 never 타입으로 추론 됩니다.


```typescript
type Dog = {
  name: string;
  color: string;
};

type Person = {
  name: string;
  language: string;
};

type Intersection1 = Dog & Person;

let a: Intersection1 = {
  name: "jsw",
  language: "ko",
  color: "red",
}
let b: Intersection1 = {
  name: "all",
  language: "us",
}    // error 
```
<div align="center">
 <img src="https://github.com/user-attachments/assets/43d820b1-575e-499b-abba-f7766b9c329e" width="50%" height="50%" title="Union 타입 집합" alt="Union 타입 집합 drawio"></img>
</div>

B 부분에 해당하며 `Dog`와 `Person` 타입 객체의 모든 프로퍼티를 가집니다.(Dog와 Person 두 객체의 서브 타입)
