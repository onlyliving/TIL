# Core Components

-   core components : https://reactnative.dev/docs/components-and-apis

-   React Native는 display 속성이 기본적으로 flex
-   예제 코드 : https://github.com/kimik-hyum/react-native-web/tree/styled-component

## Basic Components

-   `<View>`

    -   UI 구축을 위한 가장 기본적인 컴포넌트
    -   flexbox, 스타일, 일부 터치 처리 및 접근성 컨트롤이 있는 레이아웃을 지원하는 컨테이너
        -   Android view : `<ViewGroup>`
        -   iOS view : `<UIView>`
        -   Web : `<div>`

-   `<Text>`

    -   텍스트를 표시하기 위한 컴포넌트
    -   스타일 지정 및 중첩하고 터치 이벤트도 처리
        -   Android view : `<TextView>`
        -   iOS view : `<UITextView>`
        -   Web : `<p>`
    -   속성 : [Props](#Text-Props)

    ```js
        // BAD: will raise exception, can't have a text node as child of a <View>
        <View>
            Some text
        </View>

        // GOOD
        <View>
            <Text>
                Some text
            </Text>
        </View>
    ```

-   `<TextInput>`

    -   텍스트 입력 구성 요소
        -   Android view : `<EditText>`
        -   iOS view : `<UITextField>`
        -   Web : `<input type="text">`

    ```js
    <TextInput style={styles.input} onChangeText={onChangeNumber} value={number} placeholder="useless placeholder" keyboardType="numeric" />
    ```

    -   기본 속성

        -   multiline
        -   onChangeText

            -   텍스트가 입력될 때 onChangeText 이벤트 처리기 실행함.

            ```js
            /**
                함수 시그니처 : 함수 선언문에서 함수 이름만 제외한 부분을 말함. 함수 타입(함수 시그니처)
                onChangeText(text: string) => void
            **/

            import React from "react";
            import { TextInput } from "react-native";

            export default function App() {
                return (
                    <>
                        <TextInput
                            placeholder="enter your name"
                            onChangeText={(text: string) => console.log(text)}
                            onFocus={() => console.log("onFocus")}
                            onBlur={() => console.log("onBlur")}
                            onEndEditing={() => console.log("onEndEditing")}
                        />
                    </>
                );
            }
            ```

        -   onSubmitEditing
        -   onFocus
            -   텍스트를 입력할 수 있는 상태(포커스를 가진 상태)가 되면 onFocus 이벤트 호출.
        -   onBlur
            -   텍스트를 입력할 수 없는 상태가 되면 onBlur 이벤트 호출.
        -   onEndEditing
            -   텍스트 입력이 모두 끝나면 onEndEditing 이벤트 호출.
        -   자식 요소를 가지지 못함.
        -   [속성 더보기](https://reactnative.dev/docs/textinput#props)

-   **이미지 관련 컴포넌트**

    -   반드시 width, height 스타일 속성값을 설정해야 함.
        -   `{width: '100%', height: '100%'}`로 설정한다면 `{flex: 1}`을 사용하여 값을 간결하게 설정할 수 있음
    -   source 속성
        -   내부 이미지 파일 : `source={require('./root/img.jpg')}`
        -   외부 이미지 파일 : `source={{uri: '이미지파일주소'}}`
    -   `<Image>`

        -   다양한 유형의 이미지를 표시하기 위한 컴포넌트
            -   Android view : `<ImageView>`
            -   iOS view : `<UIImageView>`
            -   Web : `<img>`

    -   `<ImageBackground>`
        ```JSX
        <ImageBackground style={{flex: 1}} source={require('./src/assets/images/bg.jpg')} />
        ```
        -   ImageBackground 컴포넌트는 이름에 'View'가 없지만 'View'자가 들어간 컴포넌트처럼 자식 컴포넌트를 가질 수 있음.

-   `<ScrollView>`

    -   여러 구성 요소와 보기를 포함할 수 있는 일반 스크롤 컨테이너
    -   스크롤이 필요한 레이아웃 요소에 컨테이너로 사용되는 컴포넌트.
    -   가로 스크롤 사용시 `horizontal` 속성으로 사용 가능.
        -   Android view : `<ScrollView>`
        -   iOS view : `<UIScrollView>`
        -   Web : `<div>`

-   `<StyleSheet>`

    -   CSS 스타일시트와 유사한 추상화 레이어 제공.

-   `<Button></Button>`

    -   필수 속성 : `title`
    -   HTML에서의 button 태그와는 다르게 title 속성으로 버튼에 표시할 텍스트를 정함.
    -   style로 font-size나 크기 등을 변경할 수 없으며, color 속성으로 버튼 자체 색을 정하는 것 외에는 커스텀 되지 않음.

    ```jsx
        <Button title="home" color="blue" onPress={() => console.log('home pressed.')}>
    ```

-   `<TouchableOpacity></TouchableOpacity>`

    -   커스텀해야 하는 버튼이나 영역을 만들고자 할 때 사용되는 컴포넌트.
    -   flex 요소
    -   터치 시 opacity가 0에서 1로 변하는 효과.
    -   `TouchableWithoutFeedback`은 피드백 없는 터치요소에 사용됨.
    -   `TouchableHighlight` 컴포넌트도 있음.
        -   `TouchableOpacity` 컴포넌트와 함께 나타나는 특징 : 단 한개의 자식 컴포넌트만 올 수 있음.

-   List Views

    -   `<FlatList>` 또는 `<SectionList>`
    -   단순 나열이면 `FlatList`, 제목과 내용 세션으로 분리되어야 하면 `SectionList`
    -   `<FlatList>` : 시간이 지남에 따라 항목 수가 변경될 수 있는 긴 데이터 목록에 적합. 모든 요소가 한 번에 표시되지 않고 현재 화면에 표시되는 요소만 렌더링 함.

        -   **[Required]** Props
            -   `data` : 목록에 대한 정보의 소스
            -   `renderItem({ item, index, separators });` : 소스에서 하나의 항목을 가져오고 렌더링할 형식이 지정된 구성 요소를 반환.

    -   `<SectionList>` : 섹션화된 목록을 렌더링.
        -   props
            -   `sections`
            -   `renderItem`
            -   `renderSectionHeader`
            -   `keyExtractor`

### Text Props

-   press event
    -   `onPress`
    -   `onLongPress`
-   `numberOfLines`: 텍스트 말줄임에 사용.
    -   default : 0
    -   `ellipsizeMode`와 같이
        사용.
    -   `<Text>` 위에 `<View>` 부모가 있어서 style width를 설정해야 원하는 ui를 얻을 수 있음
-   `TextLayoutEvent`
    -   TextLayoutEvent 객체는 구성 요소 레이아웃 변경의 결과로 콜백에서 반환됨
    ```json
    // e.g.
    {
        "capHeight": 10.496,
        "ascender": 14.624,
        "descender": 4,
        "width": 28.224,
        "height": 18.624,
        "xHeight": 6.048,
        "x": 0,
        "y": 0
    }
    ```
-   `selectable`
    -   default : false
    -   사용자가 텍스트를 선택하여 기본 복사 및 붙여넣기 기능을 사용할 수 있습니다
-   `style`
    -   [Text Style](https://reactnative.dev/docs/text-style-props)
    -   [View Style Props](https://reactnative.dev/docs/view-style-props)
