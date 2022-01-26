# `React Native Web` 설치 및 실행

## 서론

-   [[React Native Web] 앱과 웹을 한번에 개발하기](https://minify.tistory.com/28) 블로그 내용을 기반으로 공부해 보려고 합니다.

-   react native web을 공부하려는 이유 : 서비스를 웹과 앱 둘 다 만들어야 하기 때문입니다. `react native`에서 `web`도 개발할 수 있다고 하니, 반가운 마음도 들었습니다.

### iOS 설치 및 실행

1. react native cli 설치

```
$ npm install -g react-native-cli
```

2. react native cli 프로젝트 생성

```
$ npx react-native init {프로젝트명}
```

3. 실행

```
# 위에서 생성한 프로젝트로 접속
$ cd { 프로젝트명 }

# 프로젝트 실행
$ react-native run-ios

# 또는
$ npm run ios

```

### Android 설치 및 실행

-   homebrew, watchman, JDK, Android Studio 설치

1. Homebrew 설치 : https://brew.sh/

2. node와 watch 설치

-   node version >= 12

```
$ brew install node
$ brew install watchman
```

3. JDK (Java Development Kit) 설치

-   JDK version >= 11

```
$ brew install --cask adoptopenjdk/openjdk/adoptopenjdk11
```

4. Android development environment
    - 4-1. Install Android Studio
        - [download link](https://developer.android.com/studio)
            - 설치할 때 아래와 같은 항목이 모두 체크 되어 있어야 함.
                - Android SDK
                - Android SDK Platform
                - Android Virtual Device
    - 4-2. Install the Android SDK
        - 안드로이드 스튜디오를 설치하면 기본적으로 최신 안드로이드 SDK가 설치됨.
        - 리엑트 네이티브 앱을 빌드하려면 Android 10(Q) 이 요구된다. (android studio의 SDK Manager를 통해 추가 Android SDK를 설치할 수 있음)
            - android studio - `Configure` - `SDK Maganer`
            - 설정할 것이 많으므로, 이후 설정은 홈페이지에서 확인할 것 (https://reactnative.dev/docs/environment-setup)
                - section : Install the Android SDK, Configure the ANDROID_HOME environment variable)
5. 실행

```
$ npm run android
```

### React Native Web 설치 및 실행

1. web 환경에 필요한 npm modules 설치

```
$ npm install --save react-dom
$ npm install --save-dev @babel/core babel-loader @babel/preset-react
$ npm install --save-dev webpack webpack-cli webpack-dev-server html-webpack-plugin

$ npm install react-native-web
```

2. `package.json`의 script에 내용 수정

```json
"scripts": {
    "android": "react-native run-android",
    "ios": "react-native run-ios",
    "start": "react-native start",
    "build-react": "webpack --mode production",
    "start-react": "webpack serve --config ./webpack.config.js --mode development",
    "test": "jest",
    "lint": "eslint ."
  },
```

3. `webpack.config.js` 파일 생성

```js
const path = require("path");
const HTMLWebpackPlugin = require("html-webpack-plugin");

const HTMLWebpackPluginConfig = new HTMLWebpackPlugin({
    template: path.resolve(__dirname, "./public/index.html"),
    filename: "index.html",
    inject: "body",
});

module.exports = {
    entry: path.join(__dirname, "index.web.js"),
    output: {
        filename: "bundle.js",
        path: path.join(__dirname, "/build"),
    },
    resolve: {
        alias: {
            "react-native$": "react-native-web",
        },
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules\/(?!()\/).*/,
                use: {
                    loader: "babel-loader",
                    options: {
                        presets: ["@babel/preset-react"],
                    },
                },
            },
        ],
    },
    plugins: [HTMLWebpackPluginConfig],
    devServer: {
        open: true,
        historyApiFallback: true,
        hot: true,
    },
};
```

4. `index.html` 파일 생성 (root : ./public/index.html)

```html
<!DOCTYPE html>
<html>
    <head>
        <title>React Native Web</title>
    </head>
    <body>
        <div id="app"></div>
    </body>
</html>
```

5. `index.web.js` 파일 생성

```js
import React from "react";
import ReactDOM from "react-dom";

import App from "./App.web";

ReactDOM.render(<App />, document.getElementById("app"));
```

6. `App.web.js` 파일 생성

```js
import React from "react";
import { View, Text } from "react-native";

function App() {
    return (
        <View>
            <Text>Hello world from react</Text>
        </View>
    );
}

export default App;
```

-   js 파일 이름은 플랫폼 분기에 따라서 이름을 구분할 수 있습니다.
    -   네이티브와 웹을 같이 쓰는 경우 : `**.js`
    -   웹에서만 사용 : `**.web.js`
    -   네이트브에서만 사용 : `**.native.js`
    -   iOS에서만 사용 : `**.ios.js`
    -   android에서만 사용 : `**.android.js`

7. 실행

```
$ npm run start-react
```

---

## Error issues

### 1. error message : `iOS build failed`

-   에러 발생 시점 : iOS 코드 빌드시 에러 발생
    ```
    ** BUILD FAILED **
    The following build commands failed:
    PhaseScriptExecution [CP-User]\ Generate\ Specs /Users/soheekim/Library/Developer/Xcode/DerivedData/rnwClient-dpifrlulsuxzeqeloylziiyjjrgo/Build/Intermediates.noindex/Pods.build/Debug-iphonesimulator/FBReactNativeSpec.build/Script-5510250E5B812F0A9E69AA498CA206A2.sh (in target 'FBReactNativeSpec' from project 'Pods')
    ```
-   비슷한 이슈 관련 내용 : https://github.com/react-native-community/upgrade-support/issues/138

-   **해결 방법**
    > In case anyone wants to try a temporary fix that worked for me: I ended up commenting all lines inside node_modules/react-native/scripts/find-node.sh. It seems as if my node environment didn't agree with this script. I could build and run my project on an actual device afterwards without any further issues.
          - find-node.sh 파일에서 명령어 모두 숨김처리 함.
              - file root : node_modules/react-native/scripts/find-node.sh

### 2. error message : `Signing for {프로젝트 폴더명} requires a development team`

-   에러 발생 시점 : xcode 실행시 에러

```
Signing for {프로젝트 폴더명} requires a development team. Select a development team in the Signing & Capabilities editor.
```

-   에러 원인 : 프로젝트에 개발자(개발팀) 서명이 필요 이슈
-   해결 관련 블로그 : https://velog.io/@kekeke257/Xcode-development-team-%ED%95%B4%EA%B2%B0

-   **해결 방법**
    -   Step 1. xcode menu
        -   Preferences - Accounts - Apple ID 작성
    -   Step 2. xcode project tab
        -   Signing & Capabilities - team 에서 설정

### 3. error message : **`Could not launch “rnwClient”`**

-   에러 발생 시점 : xcode 실행시 빌드 성공 후에 앱 신뢰 관련 이슈
-   **해결방법**
    -   아이폰 설정 - VPN 및 기기 관리 - 개발자 앱 - 신뢰 설정
