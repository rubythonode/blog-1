title: 라즈베리파이 사용기-1
date: 2015-12-20 19:15:21
categories:
- Raspberrypi
tags:
- raspberrypi
- 라즈베리파이
---
나중에 필요할 때 참고할 수 있도록 라즈베리파이(이하 산딸기)를 사용하는 흔적들을 남기려 합니다.
주로 무언가 설치하거나 설정하는 방법들에 대해서 기록할 것입니다.
<!-- more -->
## OS 설치
산딸기에 OS를 설치하는 방법으로 크게 두 가지가 있습니다.
- OS 이미지를 SD카드에 옮겨 설치
- NOOBS를 이용하여 설치

NOOBS는 산딸기를 위한 OS를 손쉽게 설치할 수 있도록 도와주는 툴입니다. NOOBS를 이용하면 Raspbian, Pidora, OpenElec, OSMC 등 산딸기에 많이 쓰이는 OS들을 간단하게 선택해서 설치할 수 있습니다.
[링크](https://www.raspberrypi.org/downloads/noobs/) 에서 내려받아 이용할 수 있습니다.

저는 NOOBS를 이용해 Raspbian을 설치했습니다.

## 초기 세팅
설치 후 기본적으로 라즈비안을 쓰기 위한 세팅을 합니다.

### 사용자 계정을 관리
 라즈비안을 설치하면 기본 계정으로
- ID: pi
- PW: raspberry

인 계정이 있습니다.
산딸기를 서버 등으로 사용하지 않고 로컬에서만 사용한다면 큰 문제 없겠지만, 그렇지 않다면 보안상의 이유로 다른 계정을 만들어서 사용하거나 패스워드를 변경하여 사용해야 합니다. 저는 별도로 계정을 만들어서 진행하였습니다.
``` bash
$ sudo useradd -m green
```
useradd 명령어로 'green' 계정을 생성하였습니다. 여기서 -m 옵션을 주면 자동으로 /home 디렉터리에 /home/green 디렉터리와 하위에 기본적인 디렉터리를 생성해줍니다.
그리고서 green 계정에 sudo 권한을 주기 위해 /etc/sudoers 파일을 수정해주었습니다.
``` bash
$ sudo vi /etc/sudoers
```
파일 맨 아래에 `green ALL=(ALL) NOPASSWD: ALL` 을 추가해줍니다. 덤으로 pi 계정에 권한을 없애기 위해 `pi ALL=(ALL) NOPASSWD: ALL` 줄은 삭제해줍니다.

이후 작업은 산딸기에서 직접 안 하고 ssh로 접속하여 진행하였습니다.
(기본적으로 라즈비안엔 ssh가 설치되어있어 별다른 과정 없이 ssh를 이용할 수 있습니다.)

### apt-get 업데이트 및 업그레이드
다음 명령어를 통해 데비안 계열 리눅스의 패키지 관리툴인 apt-get의 인덱스 정보를 업데이트하고, 설치된 패키지들을 새 버전으로 업그레이드합니다.
``` bash
$ sudo apt-get update
$ sudo apt-get upgrade
```

### 자주 쓰는 패키지들 설치
자주 쓰는 패키지들을 설치합니다. 저는 일단 vim, nmap, tree, bash-completion 을 설치했습니다.
``` bash
$ sudo apt-get install -y vim namp tree bash-completion
```
