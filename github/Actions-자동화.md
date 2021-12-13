# Github Actions

-   GitHub의 소프트웨어 개발 워크플로에서 작업을 자동화하기 위한 패키지 스크립트
-   yml 데이터 형식을 따르는 스크립트

## 학습

-   [GitHub Actions를 사용하여 개발 작업 자동화](https://docs.microsoft.com/ko-kr/learn/modules/github-actions-automate-tasks/)

## Github Actions 예제

-   참고 블로그 : [자동으로 스터디 모집 글을 모아 알림을 주는 파이썬 크롤러 만들기(with Github Actions)](https://baek.dev/post/17/)

-   예제 활용 : [ci-cd-test](https://github.com/onlyliving/ci-cd-test)

## `on.schedule`

#### `cron` : 주기적 실행을 위한 스케줄링

> 시간대 기준 : UTC (한국 표준시는 UTC보다 9시간 빠른 표준시간. UTC + 09:00)

-   [Github Docs](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions#onschedule)
-   [The quick and simple editor for cron schedule expressions](https://crontab.guru/)

```yml
# python-publish.yml
on:
    schedule:
        # * is a special character in YAML so you have to quote this string
        - cron: "30 5,17 * * *"
```

```
[minute] [hour] [day] [month] [week]

e.g. (UTC 기준)

0 9 1 * *
	-> 매월 1일 오전 9시

*/20 * * * *
	->  매 20분 간격으로 실행

30 5,17 * * *
	-> 매일 5:30, 17:30

30 10 * * *
	-> 매일 오전 10:30
```
