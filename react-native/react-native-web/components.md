# React Native Components

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
        -   onChangeText
        -   onSubmitEditing
        -   onFocus
        -   multiline
        -   [속성 더보기](https://reactnative.dev/docs/textinput#props)

-   `<Image>`

    -   다양한 유형의 이미지를 표시하기 위한 컴포넌트
        -   Android view : `<ImageView>`
        -   iOS view : `<UIImageView>`
        -   Web : `<img>`

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

    -   HTML에서의 button 태그와는 다르게 title 속성으로 버튼에 표시할 텍스트를 정함.
    -   style로 font-size나 크기 등을 변경할 수 없으며, color 속성으로 버튼 자체 색을 정하는 것 외에는 커스텀 되지 않음.

-   `<TouchableOpacity></TouchableOpacity>`

    -   커스텀해야 하는 버튼이나 영역을 만들고자 할 때 사용되는 컴포넌트.
    -   flex 요소
    -   터치 시 opacity가 0에서 1로 변하는 효과.
    -   `TouchableWithoutFeedback`은 피드백 없는 터치요소에 사용됨.

-   목록 표시
    -   `<FlatList>` 또는 `<SectionList>`
    -   `<FlatList>` : 시간이 지남에 따라 항목 수가 변경될 수 있는 긴 데이터 목록에 적합. 모든 요소가 한 번에 표시되지 않고 현재 화면에 표시되는 요소만 렌더링 함.
        -   `<FlatList>` 속성 : `data`, `renderItem`
            -   `data` : 목록에 대한 정보의 소스
            -   `renderItem` : 소스에서 하나의 항목을 가져오고 렌더링할 형식이 지정된 구성 요소를 반환.

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
