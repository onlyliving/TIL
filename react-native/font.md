# React Native Custom Font

-   커스텀 폰트 적용하기
-   참고 블로그 : https://velog.io/@ziyoonee/React-Native-custom-font-%EC%A0%81%EC%9A%A9

### 1. `react-native.config.js` 파일 생성

```js
module.exports = {
    assets: ["./src/assets/fonts"], // 폰트 폴더 경로
};
```

### 2. 폰트를 프로젝트에 연결하기

```bash
$ react-native link
```

### 3. 폰트 설정 확인하기

#### `iOS`

-   `Info.plist` 파일 확인하기

### `android`

-   `src/main/assets/fonts` 폴더에 파일이 생성된 것 확인하기
