title: 라즈베리파이 사용기-1
date: 2015-12-20 19:15:21
categories:
- raspberrypi
tags:
- raspberrypi
- 라즈베리파이
---
나중에 필요할 때 참고할 수 있도록 라즈베리파이를 사용하는 흔적들을 남기려 한다.
주로 무언가 설치하거나 설정하는 방법들에 대해서 기록할 것이다. 
<!-- more -->
## OS 설치
라즈베리파이에 OS를 설치하는 방법으로 크게 두 가지가 있다.

- OS이미지를 SD카드에 옮겨 설치
- NOOBS를 이용하여 설치
NOOBS는 라즈베리파이를 위한 OS를 손쉽게 설치할 수 있도록 도와주는 툴이다. Raspbian, Pidora, OpenElec, OSMC 등 라즈베리파이에 많이 쓰이는 OS들을 간단하게 선택해서 설치할 수 있다. 
[링크](https://www.raspberrypi.org/downloads/noobs/) 에서 다운받아 이용할 수 있다.

나는 NOOBS를 이용해 Raspbian을 설치했다.

## 초기 셋팅
설치 후 기본적으로 라즈비안을 쓰기위한 셋팅을 한다. 

### 사용자 계정을 관리
 라즈비안을 설치하면 기본 계정으로
- ID: pi
- PW: raspberry 

인 계정이 있다.
라즈베리파이를 서버 등으로 사용안하고 로컬에서만 사용한다면 큰 문제 없겠지만, 그렇지 않다면 보안상의 이유로 다른 계정을 만들어서 사용하거나 패스워드를 변경하여 사용한다. 나는 별도로 계정을 만들었다.
``` bash
$ sudo useradd -m green
```
useradd 커맨드로 'green' 계정을 생성하였다. 여기서 -m 옵션을 주면 자동으로 /home 디렉토리에 /home/green 디렉토리와 하위에 기본적인 디렉토리를 생성해준다.
그리고나서 green 계정에 sudo 권한을 주기위해 /etc/sudoers 파일을 수정해주었다.
``` bash
$ sudo vi /etc/sudoers
```
파일 맨 아래에 `green ALL=(ALL) NOPASSWD: ALL` 을 추가해준다. 덤으로 pi계정에 권한을 없애기 위해 `pi ALL=(ALL) NOPASSWD: ALL` 줄은 삭제해준다.

이후 작업은 라즈베리파이에서 직접 안하고 ssh로 접속하여 진행한다.

### apt-get 업데이트 및 업그레이드
다음 명령어를 통해 데비안 계열 리눅스의 패키지 관리툴인 apt-get의 인덱스 정보를 업데이트를 하고, 설치되어 있는 패키지들을 새버전으로 업그레이드한다.
``` bash
$ sudo apt-get update
$ sudo apt-get upgrade
```
