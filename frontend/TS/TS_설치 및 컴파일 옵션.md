# Typescript Setting
## 설치
- nodejs (javascript 파일을 실행)
- typescript (typescript 컴파일러)
- or tsx (typescript 컴파일러 및 javascript 실행, ts-node: deprecated 패키지)

### 1) typescript 패키지 사용
a. nodejs 초기화
> npm init

b. nodejs 타입 패키지 설치
> npm i @types/node

c. typescript 컴파일러 설치
> sudo npm i typescript -g

d. typescript 파일 실행
> tsc [ts 파일 경로] // typescript 파일 검사 및 컴파일
> node [js 파일 경로] // 컴파일 된 javascript 파일 실행

### 2) tsx 사용
a. typescript 컴파일과 실행을 동시에 수행하는 패키지
> sudo npm i tsx -g

b. typescript 파일 실행
> tsx [ts 파일 경로] // typescript 컴파일 + javascript 파일 실행


## Typescript 컴파일 옵션

typescript 옵션 파일 생성
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

주의
1. typescript는 기본적으로 전역 모듈
moduleDetection: force
- export, import 없이 typescript의 모든 파일을 독립 모듈로 사용
```typescript
const a = 1;

export {};  // 해당 모듈을 독립적으로 사용
```

2. js 모듈
모듈 시스템

3. 
