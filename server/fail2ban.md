# fail2ban

-   무작위 접속을 막는 프레임워크 (Python)
-   SSH를 공용으로 열어야 하는 경우, 보안 강화를 위해 꼭 설치하는 것을 권장합니다.
    -   ssh로 불법 접근시 자동으로 IP 차단(ban)됩니다.
    -   지정한 시간 내 지정한 횟수만큼 실패하면 지정한 시간만큼 접속을 차단시킵니다.
    -   로그기반이므로, 로그가 쌓이는 시점으로 계산됩니다.
-   fail2Ban은 잘못된 인증 시도 횟수를 줄일 수 있지만, 최소한의 보안입니다.
    -   보안을 강화하려면, 두 가지 보안 요소를 사용하거나 공용/개인 인증 메커니즘만 사용하도록 서비스를 구성해야 합니다.

### 사용 경험 / 효과

#### 문제 현상 1. 서버 부하

-   갑자기 서버에 부하가 많이 생겼는데, (재부팅으로) 복구는 했지만 그 원인을 찾을 수 없었습니다.

#### 문제 현상 2. 허술한 로그인

-   서버 접속은 ssh로 접속하고 패스워드로 인증합니다. (공개키/비밀키 사용 X)
-   예방을 위해서 `fail2ban`을 설치하였습니다. (default : 10분 동안 5회 로그인 실패시 10분 동안 해당 ip 차단)

#### `fail2ban` 설치 후 효과

-   차단된 ip 로그를 살펴보니 엄청난 횟수로 로그인을 시도한 ip를 추적할 수 있었습니다.
-   악성 봇 인지, 보안에 더욱 신경쓰게 되었습니다.

---

## fail2ban 설치 및 실행

#### 설치 조건(PREREQUISITE)

-   ubuntu 환경
-   python 2.6 또는 3.2 이상

### 1. fail2ban 설치

```bash
$ sudo apt update && sudo apt install fail2ban -y

# fail2ban 설치 확인
$ fail2ban-client -V
```

### 2. fail2ban 설정

```bash
$ sudo vi /etc/fail2ban/jail.conf
```

-   [자세한 사항은 아래에](#fail2ban-설정)

### 3. fail2ban 실행

---

```bash
# (fail2ban 서비스를 시작하기전) 방화벽 서비스 시작 (꼭 해야 하나?)
$ systemctl enable firewalld
$ systemctl start firewalld

# fail2ban 서비스 시작
$ sudo systemctl start fail2ban

# fail2ban service 활성화(부팅 시 자동 시작)
$ sudo systemctl enable fail2ban
```

---

## fail2ban 사용법

```bash
# fail2ban 상태 확인
$ sudo fail2ban-client status

# fail2ban 상태에서 특정 서비스(sshd) 상태 확인
$ sudo fail2ban-client status sshd

# fail2ban 설정 변경 후 서비스 재시작
$ sudo systemctl restart fail2ban
# 또는
$ sudo service fail2ban restart

# 차단된 IP 로그 확인
$ sudo vi /var/log/fail2ban.log

# 수동으로 IP 차단 추가하기
$ sudo fail2ban-client set sshd hanip x.x.x.x

# 차단된 IP 복구하기
$ sudo fail2ban-client set sshd unbanip x.x.x.x

# fail2ban 중지
$ sudo systemctl stop fail2ban
```

---

## fail2ban 삭제

-   `–auto-remove` 옵션을 추가하면, 사용하지 않는 관련 package를 모두 삭제

```bash
# 설정 파일을 유지하며 fail2ban을 삭제
$ sudo apt remove --auto-remove fail2ban*

# 설정 파일과 함께 fail2ban을 삭제 (단, 사용자 홈 디렉터리의 설정 파일은 유지됨)
$ sudo apt purge --auto-remove fail2ban*
```

---

## fail2ban-설정

-   path : `/etc/fail2ban/`
-   `/etc/fail2ban/jail.conf` 파일은 업데이트 시 초기화되므로, ‘/etc/fail2ban/jail.d/\*.conf’ 또는 ‘/etc/fail2ban/jail.local’ 파일 생성 후 설정하는 것을 추천
-   설정 파일: `jail.conf` 파일에 바로 설정 가능. 패키지 업데이트 등이 진행되면 설정이 초기화될 수 있음. 기본 설정은 그대로 두고, 개별 설정을 부가적으로 적용하는 방법을 설정.
    -   기본 설정 파일 : `jail.conf`
    -   개별 설정을 부가적으로 적용할 파일 : `jail.local`

```
$ cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
$ sudo vi /etc/fail2ban/jail.local
```

**`vi jail.local`**

-   dovecot이나 postfix 처럼 기본 제공되는 filter에서 해당되는 서비스를 활성화 시켜줌.
-   sshd는 필수적으로 적용

-   `ignoreip`
    -   허가 IP 대역
    -   자신이 주로 사용하는 IP 및 내부 네트워크를 추가해 둬서, 내 IP가 차단당하지 않도록 한다.
    -   e.g. `ignoreip = 127.0.0.1/8 192.168.0.0/24`
-   `bantime`

    -   인증 실패 시 차단 시간(단위 : 초)
    -   -1로 설정 시 영구 차단
    -   e.g.
        -   24시간 : `bantime = 24h` 또는 ` bantime = 86400`

-   `findtime`

    -   입력한 시간안에 허용횟수 초과시 차단(단위 : 초)
    -   e.g.
        -   10분 : `findtime = 10m`또는 `findtime = 600`

-   `maxretry`

    -   차단되기 전까지 인증시도를 위한 허용횟수. 허용횟수 초과시 차단
    -   기본값 : 5

-   `enabled`

        -   기본적으로 모든 `jails`은 비활성화.
        -   `enabled : true`
            - true 로 설정하면 활성화되고,

    로그 파일에서 변경 사항이 모니터링 됨.

-   `backend`
    -   로그 파일 변경을 감지할 방법 : `pyinotify`, `gamin`, `polling`, `systemd, auto`
        -   `pyinotify` : pyinotify(파일 변경 모니터)를 되어 있어야 합니다. 설치되어있지 않은 경우 `auto` 사용
        -   `gamin` : Gamin(파일 변경 모니터) 설치가 되어있지 않으면 `auto` 사용
        -   `polling` : 외부 라이브러리가 필요하지 않은 polling 알고리즘 사용
        -   `systemd` : systemd python 라이브러리를 사용하여 systemd 저널에 액세스합니다. 이 백엔드에는 "logpath" 지정이 유효하지 않을 경우 사용
        -   `auto` : 다음 순서대로 동작 합니다.
            -   pyinotify, gamin, polling
-   `action`
    -   차단했을 때 실행할 액션
    -   기본설정 : `action = %(action_)s`
        -   아무 이벤트 X
    -   `action = %(action_mw)s`
        -   메일을 전송하고 whois 로 IP 정보를 조회한 결과를 첨부
    -   `action = %(action_mwl)s`
        -   권장 설정. 메일을 전송하고 whois 로 IP 정보를 조회한 결과와 관련된 로그를 첨부

```
[DEFAULT]
ignoreip = 127.0.0.1/8 192.168.201.0/24 192.168.123.0/24
bantime = 10800
findtime = 600
maxretry = 5
backend = pooling
destemail = ***@***.com #수신 메일 주소. 다중 수신은 지원하지 않음
sender = ***@***.com #발신 메일 주소. = fail2ban
mta = sendmail #메일 전송 프로그램
action = %(action_mwl)s

[sshd]
enabled = true
port = ssh,22

[dovecot]
enabled = true

[postfix]
enabled = true

[postfix-sasl]
enabled = true
```

**Log 파일 설정 수정**

-   fail2ban 은 cron 을 사용하는 logrotate 를 이용해서 로그 파일을 관리
-   기본 설정은 1주일 단위로 로그파일을 갱신
-   `/etc/logrotate.d/fail2ban`

---

## 알림 설정

-   차단된 정보를 알림 받을 수 있습니다.

#### Slack 알림 : [Send notifications to the Slack from fail2ban](https://gist.github.com/Nihisil/29fd2971c9dd109ae245)

---

**참고 링크**

-   [리눅스 서버 sshd 무작위 로그인 시도 차단하기 – fail2ban](http://nblog.syszone.co.kr/archives/10148)
-   [fail2ban 및 GeoIP 와 BlackIP를 이용한 IP 차단하기](https://iskra.sarang.net/180)
-   [
    우분투(Ubuntu) 환경에 패키지(Package)로 Fail2ban 설치하기](https://lindarex.github.io/fail2ban/ubuntu-fail2ban-installation/)

**keyword**

-   `sshd`
    -   OpenSSH SSH daemon
    -   리눅스에 SSH(보안쉘) 접속을 할 수 있게 해주는 SSH 서버 데몬
    -   설정파일 : `/etc/ssh/sshd_config`
    -   기본 포트 : 22
