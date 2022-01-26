# React Native Component

-   core components : https://reactnative.dev/docs/components-and-apis

-   React Native는 display 속성이 기본적으로 flex

## Basic Components

-   `<View></View>`

    -   UI 구축을 위한 가장 기본적인 컴포넌트.
    -   가장 많이 사용하는 컨포넌트. div와 유사.

-   `<Text></Text>`

    -   텍스트를 표시하기 위한 컴포넌트. inline 요소.

-   `<Image></Image>`

    -   이미지를 표시하기 위한 컴포넌트.

-   `<TextInput></TextInput>`

    -   키보드를 통해 앱에 텍스트를 입력하기 위한 구성 요소.

-   `<ScrollView></ScrollView>`

    -   여러 구성 요소와 보기를 호스팅할 수 있는 스크롤 컨테이너를 제공합니다.
    -   스크롤이 필요한 레이아웃 요소에 컨테이너로 사용되는 컴포넌트.
    -   가로 스크롤 사용시 `horizontal` 속성으로 사용 가능.

-   `<StyleSheet></StyleSheet>`

    -   CSS 스타일시트와 유사한 추상화 레이어 제공.

-   `<Button></Button>`

    -   HTML에서의 button 태그와는 다르게 title 속성으로 버튼에 표시할 텍스트를 정함.
    -   style로 font-size나 크기 등을 변경할 수 없으며, color 속성으로 버튼 자체 색을 정하는 것 외에는 커스텀 되지 않음.

-   `<TouchableOpacity></TouchableOpacity>`
    -   커스텀해야 하는 버튼이나 영역을 만들고자 할 때 사용되는 컴포넌트.
    -   flex 요소
    -   터치 시 opacity가 0에서 1로 변하는 효과.
    -   `TouchableWithoutFeedback`은 피드백 없는 터치요소에 사용됨.
