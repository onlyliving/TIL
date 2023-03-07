# JSX.Element vs React.ReactElement vs React.ReactNode

- `JSX.Element`, `React.ReactElement`, `React.ReactNode`를 지금까지 구분 없이 쓴 것을 반성하며 제대로 알아가는 시간을 가져보자.



## `JSX.Element` vs `ReactElement`
- 두 유형 모두 React.createElement()/ jsx() 함수 호출의 결과입니다
- 두 유형 모두 함수 컴포넌트의 render 함수가 기본적으로 리턴하는 타입
- JSX.Element는 단순히 ReactElement 인터페이스를 상속 받은 인터페이스 (거의 동일)
    - React.ReactElement >= JSX.Element

```ts
// ReactElement
interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>> {
  type: T;
  props: P;
  key: Key | null;
}


// JSX.Element
declare global {
  namespace JSX {
    interface Element extends React.ReactElement<any, any> { }
    // ...
  }
}   
```

## `React.ReactNode`
- React 노드 자체는 가상 DOM을 나타냅니다. ReactNode 구성 요소의 가능한 모든 반환 값 집합도 마찬가지 입니다.
- ReactNode 타입은 클래스 컴포넌트의 render 함수가 기본적으로 리턴하는 타입
```ts
type ReactChild = ReactElement | ReactText;

type ReactFragment = {} | Iterable<ReactNode>;

interface ReactPortal extends ReactElement {
  key: Key | null;
  children: ReactNode;
}

type ReactNode =
  | ReactChild
  | ReactFragment
  | ReactPortal
  | boolean
  | null
  | undefined;
```