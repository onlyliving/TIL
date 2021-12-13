# fail2ban

-   침입 차단 소프트웨어 프레임워크
-   컴퓨터 서버를 무차별 대입 공격으로부터 보호
-   ssh log파일을 스캔해 수상한 ip를 ban해주는 소프트웨어
-   프로그래밍 언어 : 파이썬

### 사용 경험 / 효과

#### 문제 현상 1. 서버 부하

-   갑자기 서버에 부하가 많이 생겼는데, (재부팅으로) 복구는 했지만 그 원인을 찾을 수 없었습니다.

#### 문제 현상 2. 허술한 로그인

-   서버 접속은 ssh로 접속하고 패스워드로 인증합니다. (공개키/비밀키 사용 X)
-   예방을 위해서 `fail2ban`을 설치하였습니다. (default : 10분 동안 5회 로그인 실패시 10분 동안 해당 ip 차단)

#### `fail2ban` 설치 후 효과

-   차단된 ip 로그를 살펴보니 엄청난 횟수로 로그인을 시도한 ip를 추적할 수 있었습니다.
-   악성 봇 인지, 보안에 더욱 신경쓰게 되었습니다.