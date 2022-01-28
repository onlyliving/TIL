# `styled-components`

## Installation

-   styled-components 라이브러리와 TypeScript 연동을 위한 라이브러리 설치

```bash
$ npm install --save styled-components
$ npm install --save-dev babel-plugin-styled-components @types/styled-components @types/styled-components-react-native
```

-   `babel-plugin-styled-components`

    -   필수 라이브러리 X. 디버깅시 class명을 확인하기 쉽게 함.

    ```js
    // babel.config.js
    module.exports = {
        // ...
        plugins: ["babel-plugin-styled-components"],
    };
    ```

## usage

-   `styled-components`는 전체 스타일을 관리하기 위한 `theme` 기능을 제공한다.
