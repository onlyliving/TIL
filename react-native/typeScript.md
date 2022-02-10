# TypeScript

## React Native에서 TypeScript 사용하기

### TypeScript에 필요한 라이브러리

```bash
$ npm install typescript @types/react @types/react-native --save-dev
```

### `tsconfig.json`

-   프로젝트 root 폴더에 `tsconfig.json`파일을 생성
-   compiler options :
    -   https://www.typescriptlang.org/docs/handbook/compiler-options.html
    -   https://www.typescriptlang.org/tsconfig

```json
{
    "compilerOptions": {
        "allowJs": true,
        "allowSyntheticDefaultImports": true,
        "esModuleInterop": true,
        "isolatedModules": true,
        "jsx": "react",
        "lib": ["es6", "es2017"],
        "moduleResolution": "node",
        "noEmit": true,
        "strict": true,
        "target": "es6",
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true
    },
    "exclude": ["node_modules", "babel.config.js", "metro.config.js", "jest.config.js"]
}
```

## TS 문법

### 타입을 구축하기 위한 구문

[참고 문서 보기](https://typescript-kr.github.io/docs/handbook/typescript-in-5-minutes.html)

1. `interface`
2. `Types`
    - `interface`를 우선적으로 사용하고, 특정 기능이 필요할 때 `type`을 사용해야 합니다.

### 원시 타입

-   Javascript에서 사용할 수 있는 원시 타입 종류 : `boolean`, `bigint`, `null`, `number`, `string`, `symbol`, `object`, `undefined`
-   Typescript에서 추가된 원시 타입 종류
    -   `any`(무엇이든 허용)
    -   `unknown`(이 타입을 사용하는 사람이 타입이 무엇인지 선언했는가를 확인)
    -   `never`(이 타입은 발생될 수 없음)
    -   `void`(undefined를 리턴하거나 리턴 값이 없는 함수)

### 타입 적용 예시

```ts
/*
interface는 함수에서 매개변수와 리턴 값을 명시하는데 사용됨.
*/
interface User {
    name: string;
    id: number;
}

function getAdminUser(): User {
    //...
}

function deleteUser(user: User) {
    //...
}

type MyBool = true | false;
type WindowStates = "open" | "closed" | "minimized";
type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;

// 함수 파라미터로 array 또는 string을 받는 함수
function getLength(obj: string | string[]) {
    return obj.length;
}

type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```
