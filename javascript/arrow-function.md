# Arrow Function
- 면접을 보는데 화살표 함수에 대해서 제대로 설명을 못했다.
- 너무 당연하듯 써왔던 것을 반성하면서, 정의를 다시 살펴본다.


## 화살표 함수의 정의
- 화살표 함수 표현(`arrow function expression`)은 전통적인 함수표현(`function`)의 간편한 대안입니다. 하지만, 화살표 함수는 몇 가지 제한점이 있고 모든 상황에 사용할 수는 없습니다.

    - this나 super에 대한 바인딩이 없고, methods 로 사용될 수 없습니다.
    - new.target키워드가 없습니다.
    - 일반적으로 스코프를 지정할 때 사용하는 call, apply, bind methods를 이용할 수 없습니다.
    - 생성자(Constructor)로 사용할 수 없습니다.
    - yield를 화살표 함수 내부에서 사용할 수 없습니다.

## 왜 화살표 함수를 사용하나?
1. 짧은 함수
```js
 var elements = [
      'Hydrogen',
      'Helium',
      'Lithium',
      'Beryllium'
    ];

// 이 문장은 배열을 반환함: [8, 6, 7, 9]
elements.map(function(element) {
    return element.length;
});

// 위의 일반적인 함수 표현은 아래 화살표 함수로 쓸 수 있다.
elements.map(element => element.length); 
```
2. 바인딩 되지 않는 `this`
- 화살표 함수가 나오기 전까지는, 모든 새로운 함수는, 어떻게 그 함수가 호출되는지에 따라 자신의 this 값을 정의했습니다.
- 화살표 함수는 자신의 this가 없습니다. 대신 화살표 함수를 둘러싸는 렉시컬 범위(lexical scope)의 this가 사용됩니다; 화살표 함수는 일반 변수 조회 규칙(normal variable lookup rules)을 따릅니다. 때문에 현재 범위에서 존재하지 않는 this를 찾을 때, 화살표 함수는 바로 바깥 범위에서 this를 찾는것으로 검색을 끝내게 됩니다.
    - `렉시컬 범위(lexical scope)`: 함수를 어디에 선언하였는지에 따라 결정되는 것. 함수의 선언에 따라 결정.


## 참고 문서
- [mdn web docs - 화살표 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
