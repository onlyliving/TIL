# TypeScript

> React Native에서 TypeScript 사용하기

## TypeScript에 필요한 라이브러리

```bash
$ npm install typescript @types/react @types/react-native --save-dev
```

## `tsconfig.json`

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
