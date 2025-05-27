# Typescript Setting
## 설치
- nodejs (javascript 파일을 실행)
- typescript (typescript 컴파일러)
- or tsx (typescript 컴파일러 및 javascript 실행, ts-node: deprecated 패키지)

### 1) typescript 패키지 사용
a. nodejs 초기화
> npm init

b. nodejs 타입 패키지, typescript 컴파일러 설치
> npm i @types/node
> sudo npm i typescript -g

c. typescript 파일 실행
> tsc [.../ts 파일] // typescript 파일 검사 및 컴파일
> node [.../js 파일] // 컴파일 된 javascript 파일 실행

### 2) tsx 사용
a. typescript 컴파일과 실행을 동시에 수행하는 패키지
> sudo npm i tsx -g

b. typescript 파일 실행
> tsx [.../ts 파일] // typescript 컴파일 + javascript 파일 실행


## Typescript 컴파일 옵션

typescript 설정 파일 생성
> tsc init

```json
// tsconfig.json
{
 "compilerOptions": {   // 컴파일하는 js 설정
  "target": "ES5",      // js 버전 설정
  "module": "ESNext",   // js 모듈 설정(CJS, ES module)
  "outDir": "dist",     // 컴파일한 파일 저장 경로
  "strict": true,       // 타입 검사 엄격 여부(매개 변수 타입 명시 등)
  "moduleDetection": "force",  // 모듈 취급 옵션
  "skipLibCheck": true, // 타입 선언 파일(.d.ts 확장자를 갖는 파일)의 유형 검사를 생략하는 옵션입니다. 20버전 이후 필수
 },
 "include": ["src"],    // 컴파일할 범위 지정, 컴파일 명령시 경로 생략 가능
ㅅ "ts-node": {
  "esm": true,          // ts-node에서 ESModule 사용 여부 -> 주의할점 > ES module 확인
 }
}
```

---
### Typescript 작성시 주의
- typescript는 기본적으로 모든 파일을 하나의 모듈로(전역 모듈) 취급해서 다른 파일에서도 같은 이름의 변수나 함수를 사용할 수 없다
```typescript
// exam.ts
const a = 1;

console.log(a);
```
```typescript
// exam2.ts
const a = 'temp'; // error, Cannot redeclare block-scoped variable 'a'.ts(2451)

console.log(a);
```

a) `export`, `import` 사용
- `export {}` 구문을 사용하면 해당 파일을 독립적으로 사용
```typescript
// exam.ts
const a = 1;

console.log(a);
export {};
```
```typescript
// exam2.ts
const a = 'temp';

console.log(a);
```

b) typescript 옵션 변경
- `moduleDetection: "force"` 옵션은 모든 파일을 각각 독립된 모듈로 사용하는 옵션
```json
{
 compilerOptions: {
  moduleDetection: "force",
  ...
}
```
```typescript
// exam.ts
const a = 1;

console.log(a);
```
```typescript
// exam2.ts
const a = 'temp'; // ok

console.log(a);
```
