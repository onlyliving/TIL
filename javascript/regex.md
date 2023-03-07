# Regular Expression

-   정규 표현식 (정규식)
-   문자열에서 특정 문자 조합을 찾기 위한 패턴
-   정규식 연습 사이트

-   정규표현식 : `/pattern(찾고 싶은 문자열)/flag`

    -   flag (검색 방식 설정)
        -   i: 대소문자를 구별하지 않고 패턴과 일치하는 결과가 있을 경우 첫 번째 결과 하나만 반환.
        -   g: 패턴과 일치하는 문자열 전역 검색. 이것이 없으면 패턴과 일치하는 첫 번째 결과만 반환된다.
        -   m: 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

-   참고 사이트
    -   정규표헌식 연습 사이트
        -   https://regexr.com/
        -   https://regexper.com/
    -   [정규표현식 참고 블로그](https://velog.io/@hustle-dev/JavaScript-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D)

## Example

```js
// 영문 소문자와 숫자만 포함.시작(^)과 끝(+$)이 있어서 들어오는 값들 중 다른 값(영문 대문자 등)이 들어오게 되면 false.
/^[a-z0-9]+$/.test("1hello1world") // true

// 영문 대소문자 포함. 숫자 option
/[a-zA-Z]+([0-9]?)+$/.test("hello"); // true (영문 소문자)
/[a-zA-Z]+([0-9]?)+$/.test("hellO"); // true (영문 소문자 + 영문 대문자)
/[a-zA-Z]+([0-9]?)+$/.test("Hhello12"); // true (영문 소문자 + 영문 대문자 + 숫자)
/[a-zA-Z]+([0-9]?)+$/.test("1Hello1world"); // true (영문 소문자 + 영문 대문자 + 숫자)
/[a-zA-Z]+([0-9]?)+$/.test("Hhello!"); // false (영문 소문자 + 영문 대문자 + 특수문자(!))

// 영문 대소문자 포함. 숫자 option. 특수문자(.) option.
/[a-zA-Z]+([0-9\.]?)+$/.test("Hhello."); // true (영문 소문자 + 영문 대문자(option) + 특수문자(.)(option))

// 영문 소문자 포함(영문 소문자로 시작). 숫자 option. 특수문자(.-_) option.
// 아래 두 정규식은, 맨 앞에 영문 소문자가 아닌 숫자나 특수문자가 들어오면 false..
/^([a-z][a-z0-9\.\-\_]+)$/.test("hello"); // true
/^([a-z][a-z0-9\.\-\_]+)$/.test("he.l1lo"); // true
/^([a-z][a-z0-9\.\-\_]+)$/.test("123hello"); // false
/^([a-z][a-z0-9\.\-\_]+)$/.test("heLlo"); // false

// 영문 소문자 포함. 숫자 option. 특수문자(.-_) option.
/^(?=.*[a-z])([a-z0-9\.\-\_]?)+$/.test("hello"); // true
/^(?=.*[a-z])([a-z0-9\.\-\_]?)+$/.test("e.l1lo"); // true
/^(?=.*[a-z])([a-z0-9\.\-\_]?)+$/.test("123hello"); // true
/^(?=.*[a-z])([a-z0-9\.\-\_]?)+$/.test("heLlo"); // false


```

-   이메일 정규식
    -   format : `{A 정규식}@{B 정규식}.co`
    -   A 정규식 : 영문 대소문자 1개 이상, 숫자 또는 특수문자(.) 넣을 수 있음
    -   B 정규식 : 영문 소문자 1개 이상, 숫자 또는 특수문자(.-\_) 넣을 수 있음
    ```js
    /([a-zA-Z]+([0-9\.]?)+)@((?=.*[a-z])([a-z0-9\.\-\_]?)+)\.co$/.test("hH.123@hello-world.co"); // true
    /([a-zA-Z]+([0-9\.]?)+)@((?=.*[a-z])([a-z0-9\.\-\_]?)+)\.co$/.test("1He0lo.world@1hello-world.co"); // true
    ```
-   비밀번호 정규식
    -   format :

## Character classes(문자 클래스)

-   문자와 숫자를 구분하는 것과 같이 문자 종류를 구분
-   `\`, `.`, `\cX`, `\d`, `\D`, `\f`, `\n`, `\r`, `\s`, `\S`, `\t`, `\v`, `\w`, `\W`, `\0`, `\xhh`, `uhhhh`, `\uhhhhh`, [\b]

    -   `[xyz]`, `[a-c]`

        -   포함된 문자 중 하나와 일치. 하이픈(-)을 사용하여 문자 범위를 지정할 수 있음.
        -   e.g.
            -   `[abcd]`는 `[a-d]`와 동일. a, b, c, d 중 하나와 일치.
            -   `[abcd-]` 하이픈이 대괄호 안에 처음 또는 끝에 위치하는 경우, 범위가 아닌 일반 문자로 인식된다.
                -   a, b, c, d, - 중 하나와 일치.
            -   `[\w-]`는 `[A-Za-z0-9_-]` 와 동일.

    -   `[^xyz]`, `[^a-c]`

        -   부정 또는 보완 문자 클래스. 대괄호로 묶이지 않은 모든 항목과 일치.
        -   `^` 문자는 입력 시작을 나타낼 수 있지만, 대괄호 안에 들어갈 경우에는 부정문!

    -   `.`

        -   줄 종결자를 제외한 모든 단일 문자와 일치.
        -   e.g.
            -   "yes make my day"라는 문자열에서 `/.y/`는 "my", "ay"는 매칭이 되지만, "yes"는 매칭되지 않음.

    -   `\d`

        -   모든 숫자와 일치. `[0-9]`와 동일.

    -   `\D`
        -   숫자가 아닌 모든 문자와 일치. `[^0-9]`와 동일
    -   `\w`
        -   언더스코어(_)를 포함하여 모든 영숫자 문자와 일치. `[A-Za-z0-9_]`와 동일
    -   `\W`
        -   `[^A-Za-z0-9_]`와 동일. "50%"의 "%"와 일치
    -   `\s`
        -   공백, 탭, 양식 공급, 줄 바꿈 및 기타 유니코드 공백을 포함하여 단일 공백 ​​문자와 일치.
        -   e.g.
            -   `/\s\w*/` 는 "foo bar"에서 " bar"와 일치.
    -   `\S`

        -   공백 이외의 단일 문자와 일치 `[^/s]`와 동일
        -   e.g.
            -   `/\S\w*/`는 "foo bar"에서 "foo"와 일치.

    -   `\`
        -   문자를 특수문자로 처리.
        -   e.g.
            -   `/b/`는 문자 "b"와 일치. `/\b/`는 문자가 단어 경계 일치를 의미.
        -   특수문자를 문자로 처리.
        -   e.g.
            -   `/a*/`는 0개 이상의 "a"와 일치함.
            -   `/a\*/`는 "a\*"와 일치.
        -   `\`를 검색하려면, `/\\/`를 사용.
    -   `x|y`

        -   "x" 또는 "y"와 일치
        -   [abc]는 `(?:a|b|c)`와 기능적으로 동일

    -   `/t`
        -   수평탭
    -   `/n`
        -   줄바꿈 코드

## Assertions(어설션)

-   줄과 단어의 시작과 끝을 나타내는 경계와 어떤 식으로든 일치가 가능함을 나타내는 기타 패턴
-   `^`, `$`, `x(?=y)`, `x(?!y)`, `(?<=y)x`, `(?<!y)x`, `\b`, `\B`

    -   `^`
        -   입력의 시작과 일치.
        -   e.g.
            -   `/^A/`는 "an A"의 "A"와 일치하지 않음. "An A"의 첫 번째 "A"와 일치.
    -   `$`
        -   입력의 끝과 일치.
        -   e.g.
            -   `/t$`는 "eater"의 "t"와 일치하지는 않지만, "eat"의 "t"와 일치.
    -   `\b`
        -   단어 경계와 일치. 문자와 공백 사이와 같이 단어 문자 뒤에 다른 단어 문자가 오지 않는 위치입니다. 일치하는 단어 경계는 일치에 포함되지 않습니다. 즉, 일치하는 단어 경계의 길이는 0입니다
        -   e.g.
            -   `/\bm/`는 "moon"의 "m"과 일치
            -   `/oo\b/`는 "moon"에서 "oo" 뒤에 "n"이 오기 떄문에 일치하지 않음.
            -   `/oon\b/`는 "moon"에서 "oon"이 문자열의 끝이므로 일치.
            -   `/\w\b\w/`는 어떤 것과도 일치하지 않음. 단어 문자 뒤에는 비단어 문자와 단어 문자가 함께 올 수 없기 때문.
    -   `\B`

        -   비단어 경계와 일치. 두 글자 사이 또는 두 공백 사이와 같이 둘 다 단어이거나 둘 다 단어가 아니어야 함. 문자열의 시작과 끝은 단어가 아닌 것으로 간주.
        -   e.g.
            -   `/\Bon/`는 "at noon"의 "on"과 일치
            -   `/ye\B/`는 "possibly yesterday"의 "ye"와 일치.

    -   `x(?=y)`
        -   "x" 다음에 "y"가 오는 경우에만 "x"와 일치
    -   `x(?!y)`
        -   "x" 다음에 "y"가 오지 않는 경우에만 "x"와 일치
        -   e.g.
            -   `/\d+(?!\.)/`는 소숫점이 뒤에 오지 않는 경우에만 숫자와 일치.
    -   `(?<=y)x`
        -   "x" 앞에 "y"가 오는 경우에만 "x"와 일치
    -   `(?<!y)x`
        -   "x" 앞에 "y"가 오지 않는 경우에만 "x"와 일치
            -   `/(?<!-)\d+/`는 앞에 빼기 기호가 없는 경우에만 숫자와 일치.

## Groups and backreferences(그룹과 범위)

-   `(x)`, `(?:x)`, `(?<Name>x)`, `x|y`, `[xyz]`, `[^xyz]`, `\Number`

    -   `(x)`
        -   캡처 그룹: x와 일치하고 일치를 기억.
        -   e.g.
            -   /(foo)/는 "foo bar"에서 "foo"를 일치시키고 기억.
    -   `(?<Name>x)`

        -   명명된 캡처링 그룹: "x"를 일치시키고 `<Name>`에서 지정한 이름으로 반환된 일치 항목의 그룹 속성에 저장. 그룹 이름에는 꺾쇠 괄호(< 및 >)가 필요합니다.
        -   e.g.
            -   `/\((?<area>\d\d\d)\)/`
                -   전화번호에서 미국 지역 코드 추출.

    -   `(?:x)`
        -   비캡처 그룹: "x"와 일치하지만 일치 항목을 기억하지 않음. 일치하는 하위 문자열은 결과 배열의 요소([1], …, [n]) 또는 미리 정의된 RegExp 객체의 속성($1, …, $9)에서 불러올 수 없음.
    -   `\n`
        -   여기서 "n"은 양의 정수. 정규 표현식의 n 괄호와 일치하는 마지막 하위 문자열에 대한 역참조(왼쪽 괄호 계산).
        -   e.g.
            -   `/apple(,)\sorange\1/`는 "apple, orange, cherry, peach"에서 "apple, orange"와 일치
    -   `\k<Name>`
        -   `<Name>`으로 지정된 명명된 캡처 그룹과 일치하는 마지막 하위 문자열에 대한 역 참조.
        -   e.g.
            -   `/(?<title>\w+), yes \k<title>/`은 "Do you copy? Sir, yes Sir!"의 "Sir, yes Sir"와 일치

## Quantifiers(수량자)

-   수량자는 일치시킬 문자 또는 표현식의 수를 나타냄.
-   `*`, `+`, `?`, `x{n}`, `x{n,m}`

    -   `x*`
        -   선행 항목 "x"를 0회 이상 일치
        -   e.g.
            -   `/bo*/`는 "A ghost booooed"의 "boooo"와 일치
            -   `/bo*/`는 "A bird warbled"의 "b"와 일치
            -   `/bo*/`는 "A goat grunted"에서는 아무것도 일치하지 않음.
            ```js
            /bo*/.test("hello world"); // false
            /bo*/.test("hello bworldo"); // "b"와 "o"가 둘 다 하나 이상은 있어야 함.
            ```
    -   `x+`
        -   선행 항목 "x"를 1회 이상 일치. `{1,}`과 동일
        -   e.g.
            -   `/a+/`는 "candy"의 "a"와 일치.
            -   `/a+/`는 "caaaaaaandy"의 모든 "a"와 일치
            ```js
            /bo+/.test("hello world"); // false
            /bo+/.test("hello bworldo"); // false "b"와 "o"가 따로 떨어져 있으면 안됨
            /bo+/.test("hello boworld"); // true "bo"가 있어서 true
            ```
    -   `x?`
        -   선행 항목 "x"를 0회 또는 1회 일치
        -   \*, +, ? 또는 {} 바로 뒤에 사용하면 최소 횟수와 일치. 기본값은 최대 횟수와 일치.
        -   e.g.
            -   `/e?le?/`는 "angel"의 "el"과 "angle"의 "le"를 찾음.
    -   `x{n}`
        -   n은 양의 정수. 이전 항목 "x"의 "n"개 항목과 정확히 일치.
        -   e.g.
            -   `/a{2}/`는 "candy"의 a와 일치하지 않음.
            -   `/a{2}/`는 "caandy"의 모든 a와 일치.
            -   `/a{2}/`는 "caaaandy"의 처음 두 a와 일치.
            ```js
            /a{3}/.test("haaaappy"); // true, a가 총 4번 들어있지만, 처음 세개의 a와 일치
            /a{3}/.exec("hello haaaaappy"); // ['aaa', index: 7, input: 'hello haaaaappy', groups: undefined]
            ```
    -   `x{n,}`
        -   "n"은 양의 정수. 선행 항목 "x"의 "n"개 이상 일치.
        -   e.g.
            -   `/a{2,}/`는 "candy"의 "a"와 일치하지 않지만 "caandy" 및 "caaaaaaandy"의 모든 a와 일치
    -   `x{n,m}`
        -   "n"은 0 또는 양의 정수. "m"은 양의 정수. m > n은 선행 항목 "x"의 최소 "n" 및 최대 "m" 항목과 일치
        -   e.g.
            -   `/a{1,3}/`는 "cndy"에서 아무 것도 일치하지 않고 "candy"에서 "a", "caandy"에서 두 개의 "a", "caaaaaaandy"에서 처음 세 개의 "a"와 일치
            ```js
            /a{1,3}/.test("caaaaaaandy"); // true
            /a{1,3}/.exec("caaaaaaandy"); // ['aaa', index: 1, input: 'caaaaaaandy', groups: undefined]
            ```
    -   `x*?`, `x+?`, `x??`, `x{n}?`, `x{n,}?`, `x{n,m}?`

        -   `*` 및 `+`와 같은 수량자는 "탐욕적"이며, 이는 가능한 한 많은 문자열과 일치시키려고 함.
        -   `?` 수량자 뒤의 문자는 수량자를 "탐욕스럽지 않음"으로 만듭니다. 즉, 일치하는 항목을 찾으면 바로 중지.

            -   e.g.

                ```js
                /<.*>/.exec("some <foo> <bar> new </bar> </foo> thing");
                // ['<foo> <bar> new </bar> </foo>', index: 5, input: 'some <foo> <bar> new </bar> </foo> thing', groups: undefined]

                /<.*?>/.exec("some <foo> <bar> new </bar> </foo> thing");
                // ['<foo>', index: 5, input: 'some <foo> <bar> new </bar> </foo> thing', groups: undefined]
                ```

## 같이 사용할 수 있는 메서드

-   `RegExp`객체의 `exec()`, `test()`
-   `String`객채의 `match()`, `matchAll()`, `replace()`, `replaceAll()`, `search()`, `split()`

```js
const reg = /\w/;
reg.exec("apple"); // ['a', index: 0, input: 'apple', groups: undefined]
reg.test("apple"); //true

"apple".match(reg); // ['a', index: 0, input: 'apple', groups: undefined]
[..."apple".matchAll(/\w/g)];
/*
 [
    ['a', index: 0, input: 'apple', groups: undefined],
    ['p', index: 1, input: 'apple', groups: undefined],
    ['p', index: 2, input: 'apple', groups: undefined],
    ['l', index: 3, input: 'apple', groups: undefined],
    ['e', index: 4, input: 'apple', groups: undefined]
 */
```
