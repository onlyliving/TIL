# React Native API

## Alert API

-   alert 메서드 : `static alert(타이틀, 메시지)`

```js
import React from "react";
import { SafeAreaView, Alert, Button, TouchableOpacity, TouchableHighlight } from "react-native";

const onPress = () => Alert.alert("home pressed", "message");

const App = () => {
    return (
        <SafeAreaView>
            <Button title="Home" onPress={onPress} />
            <TouchableOpacity onPress={onPress}>
                <Text>TouchableOpacity</Text>
            </TouchableOpacity>
            <TouchableHighlight onPress={onPress}>
                <Text>TouchableHighlight</Text>
            </TouchableHighlight>
            <Text onPress={onPress}>Press Me</Text>
        </SafeAreaView>
    );
};

export default App;
```

-   사용할 수 있는 컴포넌트 : `Button`, `TouchableOpacity`, `HouchableHighlight`, `Text`
-   해당 컴포넌트에서 onPress 속성을 사용할 수 있음.
-   Button의 onPress는 UI를 변경할 수 없다는 단점이 있음.

## Platform API

-   현재 앱이 어느 쪽 폰에 동작하는지 체크.

```JSX
import {Platform} from 'react-native'
console.log(Platform.OS) // 'android' or 'ios'
```

-   [detail](./platform-module.md)

## Demension API

-   현재 실행된 폰의 크기 확인
-   이 두 값은 폰을 가로로 회전하더라도(landscape mode) 변하지 않음.

```JSX
import {Dimensions} from 'react-native'
const {width, height} = Demensions.get('window')
```
