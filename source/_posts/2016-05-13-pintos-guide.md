title: Pintos Project Guide
date: 2016-05-13 21:06:06
category:
- dev
- pintos
tags:
- pintos
---
- 이 포스트는 지속적으로 갱신되는 포스트입니다.
- 이 포스트는 개인적으로 Pintos 프로젝트를 진행하며 스탠포드에서 제공하는 [Pintos Project Guide](https://web.stanford.edu/class/cs140/projects/pintos/pintos.html) 페이지를 해석해둔 것을 기록하는 포스트입니다.
- 본인의 영어 실력이 부족하여 어마무시하게 많은 오역과 의역이 있습니다.
- 위와 같은 이유로 해석하지 않고 넘어가는 부분도 굉장히 많습니다.

<!-- more -->
---
## Table of Contents
1. [Introduction](#1-Introduction)
  1.1. [Getting Started](#1-1-Getting-Started)
    1.1.1. [Source Tree Overview](#1-1-1-Source-Tree-Overview)
    1.1.2. Building Pintos
    1.1.3. Running Pintos
    1.1.4. Debugging versus Testing
  1.2 Grading
    1.2.1 Testing
    1.2.2 Design
      1.2.2.1 Design Document
      1.2.2.2 Source Code
    1.3 Legal and Ethical Issues
    1.4 Acknowledgements
    1.5 Trivia
2. [Project 1: Threads](#2-Project-1-Threads)
  2.1 [Background](#2-1-Background)
    2.1.1 [Understanding Threads](#2-1-1-Understanding-Threads)
    2.1.2 [Source Files](#2-1-2-Source-Files)
      2.1.2.1 ["devices" code](#2-1-2-1-“devices”-code)
      2.1.2.2 ["lib" files](#2-1-2-2-“lib”-files)
    2.1.3 Synchronization
    2.1.4 Development Suggestions
  2.2 Requirements
    2.2.1 Design Document
    2.2.2 Alarm Clock
    2.2.3 Priority Scheduling
    2.2.4 Advanced Scheduler
  2.3 FAQ
    2.3.1 Alarm Clock FAQ
    2.3.2 Priority Scheduling FAQ
    2.3.3 Advanced Scheduler FAQ

---
## 1. Introduction
### 1.1 Getting Started
#### 1.1.1 Source Tree Overview
이제 다음 명령어를 실행해서 "pintos/src"라는 이름의 디렉토리에 Pintos source를 추출할 수 있다.`zcat /usr/class/cs140/pintos/pintos.tar.gz | tar x` 또는, http://www.stanford.edu/class/cs140/projects/pintos/pintos.tar.gz 에서 내려 받고 비슷한 방법으로 추출할 수 있다.

"pintos/src"

"threads/"
  = base kernel 소스코드, 프로젝트 1에서 이 코드를 수정할 것이다.

"userprog/"
  = user program loader 소스코드, 프로젝트 2에서 이 코드를 수정할 것이다.

"vm/"
  = 거의 빈 디렉토리. 프로젝트 3에서 여기에 virtual memory를 구현할 것이다.

"filesys/"
  = basic file system 소스코드. 프로젝트 2에서 이 file system을 사용할 것이다. 하지만 프로젝트 4 까지는 이 소스코드를 수정하지 않을 것이다.

"lib/"

"lib/kernel/"

"lib/user/"

"tests/"

"examples/"

"misc/"
"utils/"
## 2. Project 1: Threads
이 과제에서, 당신에게 최소한의 기능을 가진 thread system을 줄 것이다. 동기화 문제의 이해를 돕기 위하여, 이 thread system의 기능성을 확장하는 것이 당신이 할 일이다. 이번 과제를 하기 위해 주로 "threads" 디렉토리에서 주로 작업을 하고, 몇 가지 작업은 "devices" 디렉토리에서 하게 될 것이다. 소개는 "threads" 디렉토리로 했다. 이번 프로젝트 설명을 읽기전에 아래의 섹션을 모두 읽고 오길 권한다.
  - 1\. Introduction
  - C. Coding Standards
  - E. Debugging Tools
  - F. Development Tools

적어도 A.1 Loading 부터 A.5 Memory Allocation을, 특히 A.3 Synchronization은 훑어보기 바란다. 이번 프로젝트를 완성하기 위해서는 B.4.4BSD Scheduler 또한 읽어야 할 것이다.

### 2.1 Background
#### 2.1.1 Understading Threads
처음 할일은 초기 thread system 코드를 읽고 이해하는 것이다. Pintos 는 이미 thread creation 과 thread completion, thread 사이를 전환하는 간단한 scheduler, 기본적인 동기화(semaphores, locks, condition variables, and optimization barriers)가 구현되어있다.

몇몇 코드들은 약간 애매모호하게 보일 수도 있다. 만약 아직 base system을 컴파일하지 않고 구동시키지 않았다면, introduction에서 설명한 대로(섹션 1. Introduction을 보라) 지금 바로 해야한다.

#### 2.1.2 Source Files
##### 2.1.2.1 "devices" code
기본 threaded kernel은 "devices" 디렉토리의 이런 파일들도 포함한다.

"timer.c"
"timer.h"
  = system timer 인 ticks, 기본적으로 초당 100회. 이번 프로젝트에서 이 코드를 수정할 것이다.

"serial.c"
"serial.h"
  = 직렬 포트 드라이버,

"block.c"
"block.h"
  = block device를 위한 추상 레이어, 즉, random-access, 고정된 크기의 블럭의 배열로 구성된 disk-like devices.

#### 2.1.2.2 "lib" files
마지막으로 "lib" 와 "lib/kernel" 디렉토리에 유용한 라이브러리 루틴들이 있다. ("lib/user"는 프로젝트 2에서 다루는 user programs에서 사용할 것이다. 커널 부분은 아니다.) 여기에 몇 가지 더 자세한 내용은 다음과 같습니다.

"ctype.h"
"inttypes.h"
"limits.h"
"stdarg.h"
"stdbool.h"
"stddef.h"
"stdint.h"
"stdio.c"
"stdio.h"
"stdlib.c"
"stdlib.h"
"string.c"
"string.h"
 = C 표준 라이브러리의 일부다. 최근 C 라이브러리의 소개에 대해서 본적이 없다면, 섹션 [C.2 C99](#)를 보면 해당 정보를 알 수 있다. 안전을위해 의도적으로 빠진부분이 무엇인지 알고싶으면 섹션 [C.3 Unsafe String Functions]()을 봐라.
 
"debug.c"
"debug.h"
 = 디버깅을 위한 함수와 매크로. 섹션 [E](#)에 디버깅툴과 좀더 자세한 설명이 있다.

"random.c"
"random.h"
 = 의사 난수 생성기(Pseudo-random number generator).
 The actual sequence of random values will not vary from one Pintos run to another, unless you do one of three things: specify a new random seed value on the -rs kernel command-line option on each run, or use a simulator other than Bochs, or specify the -r option to pintos.

round.h
 = 라운딩 매크로.

"syscall-nr.h"
 = 시스템 콜 넘버. 프로젝트 2까지 사용하지 않는다.

"kernel/list.c"
"kernel/list.h"
 = Doubly linked list 구현. Pintos 코드 전반에 걸쳐 사용한다. 당신은 아마 프로젝트 1 곳곳에서 사용하게 될 것이다.

"kernel/bitmap.c"
"kernel/bitmap.h"
 = Bitmap 구현. 당신이 원한다면 당신의 코드에 사용할 수 있다. 하지만 아마 프로젝트 1에서 사용하지 않을 것이다.

"kernel/hash.c"
"kernel/hash.h"
 = Hash table 구현. 프로젝트 3에서 유용하게 쓸 것이다.

"kernel/console.c"
"kernel/console.h"
"kernel/stdio.h"
 = `printf()`와 몇몇 함수의 구현.
