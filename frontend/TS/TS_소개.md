# Typescript

> NodeJs의 등장으로 브라우저 외부에서도 Javascript를 사용할 수 있게 되었다. 서버, 모바일 앱, 데스크탑 앱에서 활용할 수 있지만 Javascript의 유연함이 앱에서의 안정성을 저하 시키게 되었다. 이 때 안정성 확보를 위해 Typescript가 등장하게 되었다


## 타입 시스템 분류
- ***어떤 기준*** 으로 타입을 규정할 것인지
- 코드 타입을 ***언제*** 검사할 것인지
- ***어떻게*** 타입을 검사할 것인지

### 1. 정적 타입 시스템(Static Type System)
: 코드 실행 전 모든 변수의 타입을 고정적으로 결정. 엄격한 시스템(C, Java)
```Java
String a = 'hi';
int b = 123;

int c = a * b; // 오류, 코드를 실행 전 오류를 확인할 수 있고 실행 되지 않는다
```

### 2. 동적 타입 시스템(Dynamic Type System)
: 코드를 실행할 때마다 유동적으로 변수의 타입을 결정. 유연한 시스템(Python, Javascript)
```javascript
let a = 12345;

a.toUpperCase(); // 오류지만 코드는 실행 가능하며 비정상적으로 종료된다
```

### 3. 점진적 타입 시스템(Gradual Type System)
: 코드를 실행하기 전에 변수의 타입을 결정하고, 타입의 오류를 검사한다. Typescript의 타입 시스템
```typescript
let a: number = 1;

a.toUpperCase(); // 코드를 실행 전 오류를 확인할 수 있고, 실행하더라도 오류 발견시 컴파일이 종료된다
```


## 타입스크립트 동작
![drawio](https://github.com/user-attachments/assets/9118a2f0-a633-43a3-9461-49e827128791)

**Javascript 컴파일**

![2 drawio (1)](https://github.com/user-attachments/assets/f4fdae71-5f1d-4585-b3db-5bce57585899)

**Typescript 컴파일**

![TS 컴파일 이미지 drawio](https://github.com/user-attachments/assets/90a56caa-37e8-4da3-baa9-75c6093d658e)


```typescript
// 오류 발견
let a: number = 1;

a.toUpperCase(); // 오류가 나타나 컴파일 종료
```
```typescript
// 오류 미발견
let b: number = 1;

console.log(b); // Javscript로 변환 후 이후 컴파일 진행
```


### Shorts
|용어|설명|
|----|----|
|컴파일|프로그래밍 언어를 컴퓨터가 이해하기 쉬운 형태로(기계어) 변환하는 작업|
|컴파일러|컴파일을 수행하는 도구|
|기계어|**CPU**가 해석하고 실행할 수 있는 0과 1로만 표현할 수 있는 비트 단위로 이루어진 컴퓨터 언어|
|바이트 코드|CPU와 같은 하드웨어가 아닌 **가상 컴퓨터**에서 동작하는 실행 프로그램을 위한 이진 표현법|
|AST(추상 문법 트리, Abstract Syntax Tree)|Javascript 코드를 실행단위인 바이트코드로 변경시키기 위한 트리 형태의 중간 자료 구조|
