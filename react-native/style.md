# React Native Style

### `요가엔진(Yoga engine)`

-   react native는 웹 브라우저에서 단순히 JS 엔진만 떼어 낸 것으로 HTML, CSS 엔진은 존재하지 않음. react native는 이 떄문에 요가(Yoga) 란 이름의 CSS 엔진을 만듦.
-   요가 엔진은 facebook의 component 배치와 스타일링을 위해 C++ 언어로 구현한 라이브러리이다.
-   웹 브라우저의 CSS 엔진과 비슷하게 동작하지만 완전 똑같지는 않다.
    -   특히 flexbox layout 부분에 차이가 있다.

### style 속성과 스타일 객체

-   컴포넌트의 style 속성에 설정되는 값은 `객체`이다.

```jsx
/**
- style에 설정한 코드 안쪽 중괄호{} 부분은 객체를 의미하는 문법.
- 바깥쪽 {} 부분은 JSX 구문에서 자바스크립트 코드를 설정할 때는 쓰는 문법.
**/

<View style={{}}></View>
```

-   스타일 속성은 소문자로 시작
-   두 단어 이상으로 된 속성 이름은 낙타 표기법을 사용.
    -   `e.g. backgroundColor`

## `인라인 스타일` 방식 VS `StyleSheet 스타일` 방식

-   컴포넌트는 필요에 따라 리액트 네이티브에 의해 재렌더링됨
    -   재렌더링은 상황에 따라 반복해서 발생
    -   이러한 상황을 고려할 때
        1. **인라인 스타일 방식**
            - 자바스크립트 엔진 쪽 스레드에서 UI 스레드 쪽으로 브리지를 경유하여 옮겨 가므로 내용이 컴포넌트 로직에 의해 바뀌지 않을 때는 앱의 디스플레이 속도가 떨어짐
            - 컴포넌트 구현 로직에 따라 동적으로 변하는 스타일 객체는 인라인 스타일 방식으로 구현하는 것이 일반적.
        2. **StyleSheet.create 방식**
            - StyleSheet.create로 생성된 스타일 객체는 UI 스레드 쪽에 캐시되므로 앱 전체의 디스플레이 속도가 빨라짐.
            - 내용이 변하지 않는 스타일(정적 스타일) 객체는 StyleSheet.create 방식으로 구현하는 것이 효과적

```JSX
/**
e.g.

아래 SafeAreaView 컴포넌트는 StyleSheet 스타일을 가졌고,
Text 컴포넌트는 인라인 스타일을 가짐
**/

import React from 'react'
import {StyleSheet, SafeAreaView, Text} from 'react-native'

export default function App() {
    return (
        <SafeAreaView style={styles.safeAreaView}>
            <Text style={{fontSize: 20, color: 'white'}}>
        </SafeAreaView>
    )

}

const styles = StyleSheet.create({
    safeAreaView: {
        flex: 1,
        alignItems: 'center',
        justifyContent: 'center',
        backgroundColor: 'blue'
    },
    text: {
        fontSize: 20
    }
})

```

### StyleSheet API

```JSX
import {StyleSheet} from 'react-native'

const styles = StyleSheet.create({
    키_이름1: 스타일_객체1,
    키_이름2: 스타일_객체2
    // ... 생략
})
```

-   `StyleSheet.create`의 목적이 JS 언어로 만든 스타일 객체를 네이티브 모듈 쪽으로 옮겨 주는 것이므로 여러 번 호출하여 스타일 객체 여러 개를 일일이 전달하는 것 보다는 한꺼번에 전달하는 것이 효율적.

---

## `뷰`가 들어간 컴포넌트에 스타일 적용

    - `View`, `ScrollView`, `SafeAreaView`, `KeyboareAvoidingView`

### `backgrounColor` 스타일 속성

-   이름에 `View`가 들어간 '뷰' 컴포넌트는 backgrounColor 스타일 속성으로 자신의 바탕색을 설정할 수 있음.

### width, height 스타일 속성

-   '뷰' 컴포넌트에 적용 가능
-   `height: '100%'` 와 동일한 스타일
    -   `height: Demensions.get('window').height`
    -   `flex: 1`

### flex와 width/height 스타일 속성을 함께 사용한다면

-   적용 우선순위 : width, height > flex

### margin

-   marginLeft=marginRight 인 경우, `marginHorizontal` 스타일 속성으로 동시에 설정 가능
-   marginTop=marginBottom 인 경우, `marginVertical` 사용.
-   상하좌우 모두 마진이 동일한 경우
    -   `e.g. margin: '10%'`
-   padding도 동일하게 적용
-   iOS에서 SafeAreaView 컴포넌트는 paddig 스타일 속성이 동작하지 않음.

### border

-   borderWidth
    -   borderLeftWidth, borderRightWidth, borderTopWidth, borderBottmWidth
-   borderColor
    -   borderLeftColor, borderRightColor, borderTopColor, borderBottomColor
-   borderRadius
    -   borderTopLeftRadius, borderTopRightRadius, borderBottomLeftRadius, borderBottomRightRadius
-   borderStyle
    -   'solid', 'dotted', 'dashed'

## 색상

### `구글 머티리얼 디자인 가이드라인`으로 색상 통일

-   구글은 모든 안드로이드 앱을 가능한 한 `구글 머티리얼 디자인 가이드라인`에 따라 디자인할 것을 권고함.
-   아이폰은 `구글 머티리얼 디자인 가이드라인`이 필요 없지만 양쪽 모두 똑같이 보이도록 하는 것이 사용자 편의를 증대할 수 있음.

#### 구글 머티리얼 색상 적용

-   `react-native-paper` 패키지를 통해서 구글 머티리얼 색상을 적용할 수 있음.

#### 색 밝기 적용

-   `color` 패키지를 통해서 가능

```JSX
import {Colors} from 'react-native-paper'
import Color from 'color'

console.log(Colors.blue500) // #2196f3
console.log(Color(Colors.blue500).alpha(0.5).lighten(0.5).string()) // hsla(206, 89.7%, 81.2%, 0.5)
// lighten 대신 darken 메서드를 사용하면 색상을 어둡게 할 수 있음
```

---

### note.

-   타입스크립트 문법에서 `height: height`처럼 속성 이름과 값ㅇ르 담은 변수 이름이 동일할 때는 값을 담은 변수 부분을 생략할 수 있는 단축 구문을 제공함.

```TS
const {width, height} = Dimensions.get('window')

const styles = StyleSheet.create({
    box: {backgroundColor: 'blue', height}
})
```
