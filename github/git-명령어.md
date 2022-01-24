# GIT 명령어

## git clone

### 모든 branch 받아오기

#### 쉘 스크립트를 이용한 방법

```linux
$ git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done

$ git fetch --all

$ git pull --all
```

-   참고 링크 : https://github.com/jobhope/TechnicalNote/blob/master/github/CloneRepository.md
