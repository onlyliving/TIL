# Shallow Copy & Deep Copy

## `Shallow Copy` 얕은 복사
- 객체의 얕은 복사본은 속성이 복사본이 만들어진 원본 객체의 참조와 동일한 참조(동일한 기본 값을 가리킴)를 공유하는 복사본입니다.
- 결과적으로 소스나 복사본 중 하나를 변경하면 다른 개체도 변경될 수 있습니다.
    - 메모리 주소만 복사가 됨.
- e.g.
    - X에 대한 참조의 복사본을 Y(= X의 주소 복사본)로 만듦.
    - X와 Y의 주소는 동일. (동일한 메모리 위치)

## `Deep Copy` 깊은 복사
- 객체의 전체 복사본은 속성이 복사본이 만들어진 원본 객체의 참조와 동일한 참조(동일한 기본 값을 가리킴)를 공유하지 않는 복사본입니다.
- 결과적으로 소스나 복사본을 변경할 때 다른 개체도 변경하지 않도록 할 수 있습니다
- e.g.
    - X의 모든 구성원을 복사하고 Y에 대해 다른 메모리 위치를 할당한 다음, 복사된 구성원을 Y에 할당하여 전체 복사를 수행. 이런 식으로 X가 사라지더라도 Y는 여전히 메모리에 유효함.
    - 둘 다 완전히 동일하지만, 메모리 공간에 두 개의 다른 위치로 저장되어 있기 떄문에 서로 다름.

## 예시

```js
var employeeDetailsOriginal = { 
    name: 'Manjula', 
    age: 25, 
    Profession: 'Software Engineer' 
};
var employeeDetailsDuplicate = employeeDetailsOriginal; //얕은 복사!
employeeDetailsDuplicate.name = 'NameChanged';
/**
 * 위의 경우, 값이 참조가 되어 있으므로, name을 employeeDetailsDuplicate에서 변경하므로, 원본 데이터도 손실됨.
 */

var employeeDetailsDuplicate = { 
        name: employeeDetailsOriginal.name, 
        age: employeeDetailsOriginal.age, 
        Profession: employeeDetailsOriginal.Profession
}; //딥 카피!

```

## 깊은 복사로 만드는 방법
- 객체를 JSON 문자열로 변환 후, 다시 객체로 변환
```js
// JSON.stringify() & JSON.parse()
// 객체를 JSON 문자열로 변환 후, 다시 객체로 변환

let ingredients_list = ["noodles", { list: ["eggs", "flour", "water"] }];
let ingredients_list_deepcopy = JSON.parse(JSON.stringify(ingredients_list));

// components_list_deepcopy에서 'list' 속성 값을 변경합니다.
ingredients_list_deepcopy[1].list = ["rice flour", "water"];

console.log(ingredients_list[1].list);
// 'list' 속성은 ingredients_list 에서 변경되지 않습니다.
// [ "eggs", "flour", "water" ]

```



### 참고 문서
- [blog - Understanding Deep and Shallow Copy in Javascript
](https://medium.com/@manjuladube/understanding-deep-and-shallow-copy-in-javascript-13438bad941c)
- [mdn web docs - Shallow copy](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy)
- [mdn web docs - Deep copy copy](https://developer.mozilla.org/en-US/docs/Glossary/Deep_copy)
