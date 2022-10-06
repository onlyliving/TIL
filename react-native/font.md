# React Native Custom Font

-   커스텀 폰트 적용하기
-   참고 블로그 : https://velog.io/@ziyoonee/React-Native-custom-font-%EC%A0%81%EC%9A%A9

### 1. 폰드 다운로드

-   Google fonts 에서 원하는 폰트를 다운로드
-   projectPath/src/assets/fonts 폴더에 다운로드 받은 폰트 넣기

### 2. `react-native.config.js` 파일 생성

```js
module.exports = {
    assets: ["./src/assets/fonts"], // 폰트 폴더 경로
};
```

### 3. `package.json` 내용 추가

```json
"rnpm":{
    "assets":[
        "./assests/fonts/"
    ]
}
```

### 4. 폰트를 프로젝트에 연결하기

```bash
$ npx react-native link
```

-   위의 명령을 실행하면 폰트가 자동으로 네이티브 폴더에 복사됨
    -   android : android/app/src/main/assets/fonts
    -   iOS : Xcode - Resources

### 5. 폰트 설정 확인하기

#### `iOS`

-   `ios/{projectName}/Info.plist` 파일 확인하기

### `android`

-   `android/app/src/main/assets/fonts` 폴더에 파일이 생성된 것 확인하기
