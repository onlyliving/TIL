# RN 프로젝트 폴더 구조

-   RN 에서는 component가 UI를 표현하는 유일한 방법이다. 화면을 구성할 때 어플의 비지니스 로직과 UI를 구분하는 것이 바로 `container`와 `presenter`이다.

-   `container`

    -   어플의 비지니스 로직
    -   해당 어플에서만 사용하는 데이터와 로직을 처리. 이것들을 props로 presenter에 넘김.
    -   Text와 Input 컴포넌트는 presenter가 가져야 하는 컴포넌트이기 떄문에 container 컴포넌트에 의존하지 않도록 주의한다.

-   `presenter`

    -   화면을 구성하는 UI
    -   비지니스 로직을 갖지 않는 것이 중요.
    -   이상적으로 presenter 컴포넌트는 인자로 props만 가지며, 비지니스 로직이 없기 때문에 앱과도 독립적이다.

-   `screens`
    -   어플의 화면단위 컴포넌트
    -   container 컴포넌트를 갖고 있다. 혹은 화면마다 겹치는 container 컴포넌트가 있다면 `containers`라는 폴더를 생성해서 관리할 수도 있다.
-   `navigation`

    -   화면 전환과 관련된 컴포넌트
    -   가장 뼈대가 되는 RootStackNavigator.js 같은 navigator 파일들이 들어감

-   `api`

    -   통신과 관련된 데이터

-   `assets`

    -   이미지와 폰트 등

-   `hooks`

    -   custom hook 파일

-   `reducers`

    -   reducer 함수

-   `theme`

    -   테마 컬러나 mixin 등 앱 전체적으로 사용할 수 있는 데이터

-   `utils`
    -   앱 전체적으로 사용 가능한 간단한 함수들

```
my-sample-app
├── build
├── node_modules
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
├── src
│   ├── assets
│   │   └──images
│   │      └── logo.svg
│   ├── components
│   │   └── app
│   │       ├── App.css
│   │       ├── App.js
│   │       └── App.test.js
│   ├── utilities
│   ├── Index.css
│   ├── Index.js
│   └── service-worker.js
├── .gitignore
├── package.json
└── README.md
```
