# Python

### `if __name__ == "__main__"`

```python
if __name__ == "__main__":
    # code...
```

-   해당 모듈이 import된 경우가 아니라 interpreter에서 직접 실행된 경우에만, if문 이하의 코드를 돌리라는 명령.

-   모듈을 실행할 수 있는 방법은 직접 실행 또는 import.

-   `__name__ == __main__` 은 interpreter에서 직접 실행했을 경우에만 if문 내의 코드를 돌리라는 명령.

-   `__name__`은 interpreter가 실행 전에 만들어 둔 글로벌 변수

#### 예시

```python
//excuteThisModule.py
def func():
    print("function working")

if __name__ == "__main__":
    print("직접 실행")
    print(__name__)
else:
    print("import되어 사용됨")
    print(__name__)
```

-   모듈을 실행 할 수 있는 방법

    -   interpreter에서 직접 실행
    -   `__name__` 변수에 `"__main__"`이 담겨서 프린트 됨.
        ```python
        python3 excuteThisModule.py
        ```
    -   다른 모듈에 import해서 실행
    -   `__name__`변수에 `"executeThisMoudle"`이 담겨서 프린트 됨.
        ```python
        # 현재 파일의 이름을 executor.py라고 해보죠.
        import executeThisMoudle.py
        executeThisMoudle.func()
        ```

-   참고 블로그 : https://medium.com/@chullino/if-name-main-%EC%9D%80-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%A0%EA%B9%8C-bc48cba7f720
