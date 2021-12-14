# fail2ban

-   무작위 접속을 막는 프레임워크
-   ssh연결 시 지정한 시간 내 지정한 횟수만큼 실패하면 지정한 시간만큼 접속을 차단시킵니다.
    -   로그기반이므로, 로그가 쌓이는 시점으로 계산됩니다.
-   로그 파일을 모니터링함으로써 작동
    -   로그 파일(예: /var/log/apache/error_log)을 검사하고 악의적인 징후를 보이는 IP를 금지합니다.
-   일반적으로 Fail2Ban은 IP 주소를 거부하도록 방화벽 규칙을 업데이트하는데 사용됩니다.
-   Fail2Ban은 잘못된 인증 시도 횟수를 줄일 수 있지만, 약한 인증으로 인한 위험을 제거할 수는 없습니다.
    -   보안을 강화하려면, 두 가지 요소를 사용하거나 공용/개인 인증 메커니즘만 사용하도록 서비스를 구성해야 합니다.
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

---

### 1. 설치

```
$ yum install -y fail2ban fail2ban-systemd whois

또는

$ apt-get install fail2ban
```

**iptables 로 차단 관리를 할 경우 firewalld 설정 제거**

```
$ rm -f /etc/fail2ban/jail.d/00-firewalld.conf
```

**fail2ban 실행**

```
$ systemctl enable fail2ban
$ systemctl start fail2ban
```

### 2. 설정

-   root : `/etc/fail2ban/`
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

설정이 완료되면 fail2ban 서비스를 재시작

```
$ systemctl restart fail2ban

또는

$ service fail2ban restart
```

fail2ban 동작 상태 확인

```
$ fail2ban-client status
```

fail2ban 동작 상태에서 특정 서비스(sshd) 상태 확인

```
$ fail2ban-client status sshd
```

차단된 IP 로그 확인

```
$ sudo vi /var/log/fail2ban.log
```

수동으로 IP 차단 추가하기

```
$ sudo fail2ban-client set sshd hanip x.x.x.x
```

차단된 IP 복구하기

```
$ fail2ban-client set sshd unbanip x.x.x.x
```

(fail2ban 서비스를 시작하기전) 방화벽 서비스 시작

```
$ systemctl enable firewalld
$ systemctl start firewalld
```

fail2ban 중지

```
$ sudo systemctl stop fail2ban
```

**Log 파일 설정 수정**

-   fail2ban 은 cron 을 사용하는 logrotate 를 이용해서 로그 파일을 관리
-   기본 설정은 1주일 단위로 로그파일을 갱신
-   `/etc/logrotate.d/fail2ban`

---

### 3. 알림 설정

-   차단된 정보를 알림 받을 수 있습니다.

#### Slack 알림 : [Send notifications to the Slack from fail2ban](https://gist.github.com/Nihisil/29fd2971c9dd109ae245)

---

**참고 링크**

-   [리눅스 서버 sshd 무작위 로그인 시도 차단하기 – fail2ban](http://nblog.syszone.co.kr/archives/10148)
-   [fail2ban 및 GeoIP 와 BlackIP를 이용한 IP 차단하기](https://iskra.sarang.net/180)

**keyword**

-   `sshd`
    -   OpenSSH SSH daemon
    -   리눅스에 SSH(보안쉘) 접속을 할 수 있게 해주는 SSH 서버 데몬
    -   설정파일 : `/etc/ssh/sshd_config`
    -   기본 포트 : 22
