# 하나의 코드로 3개의 플랫폼에 화면 출력

> 참고 블로그 : https://minify.tistory.com/29?category=704381

-   IE 설정은 skip (내가 하려는 서비스는 IE 필요 없음)

-   기본 코드는 `setting.md`를 참고할 것.

1. **`App.js`** 파일의 코드 전체 변경

```js
import React from "react";
import { Text, View, StyleSheet } from "react-native";

function App() {
    return (
        <View style={styles.container}>
            <Text>Hello world from React Naitve Web</Text>
        </View>
    );
}
const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
    },
});

export default App;
```

2. **`index.web.js`** 파일에서 `import` 부분 수정

```js
// 기존
// import App from './App.web'

// 변경
import App from "./App";
```

3. 실행

```
# web 실행
$ npm run start-react

# ios 실행
$ npm run ios

# android 실행
$ npm run android

# 명령어 한번에..
$ npm run start-react && npm run ios && npm run android
```

---

### error message : **`Task :app:installDebug FAILED`**

-   에러 발생 시점 : 안드로이드 실행 에러
-   임시 해결 : android debug bridge server restart
-   참고 링크 : https://github.com/facebook/react-native/issues/27917

```
$ adb kill-server
$ adb start-server
$ npx react-native run-android --no-jetifier
```

-   adb : Android Debug Bridge
