title: express 모듈로 nodejs app 자동 생성하기
date: 2015-12-18 00:47:56
categories:
- dev
- nodejs
tags: [express,node,nodejs]
---
nodejs를 공부하던 중 책에서 다음과 같은 명령어로 app을 자동 생성하는 부분을 보았다.

``` bash
$ express --session --ejs --css stylus myapp
```
<!-- more -->
express 모듈을 -g 옵션을 주어 글로벌로 설치한 후에 위와 같이하면 app이 자동으로 생성된다는데 아무리 해봐도 express command가 없다고만 나온다.
구글링으로 찾아보니 'express-generator' 모듈을 설치해야 커맨드창에서 express 커맨드를 사용할 수 있다고 한다.
``` bash
$ npm install -g express-generator
```
위와 같이 글로벌로 설치 후에 책에 있는 예제대로 실행하면 자동으로 nodejs app이 생성되는 것을 확인할 수 있다.
