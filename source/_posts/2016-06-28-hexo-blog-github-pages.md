title: Github pages와 Hexo를 이용하여 블로그 만들기
date: 2016-06-28 16:38:05
category:
- Blog
- Hexo
tags:
- hexo
- blog
- github
---


[지난번 포스트](/2016/02/15/hexo-init/)에서 Hexo라는 정적블로그 프레임워크에 대해 간략하게 소개했습니다.

이번엔 hexo와 github의 부가기능인 github-pages를 이용하여 별도의 서버를 두지 않고 github의 repository에 블로그 파일을 올려 블로그를 게시하는 방법에 대해 이야기해보겠습니다.

<!-- more -->

## Hexo 설치
우선 아래와 같이 npm으로 Hexo 설치 후 blog 디렉토리에 기본파일들을 설치합니다.

``` bash
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
```

## Hexo 포스트 작성
위와 같이 설치후
``` bash
$ hexo new testpost   #기본 명령어
$ hexo n testpost     #축약형 명령어  - 둘중에 편하신 명령어로 사용하시면 됩니다.
```
를 하여 테스트용 새 포스트를 작성해봅니다.

위 명령어를 실행하면 아래와 같은 디렉토리에 `testpost.md`라고 마크다운 파일이 생성됩니다.
![새 포스트 디렉토리 구조](/assets/images/post-tree.png)

새롭게 생성된 포스트 파일 `testpost.md`를 텍스트 에디터로 열어보면 다음과 같이 작성되어있습니다.

``` markdown
---
title: testpost
date: 2016-06-28 16:20:43
tags:
---

```

타이틀이 디폴트로 파일명과 똑같이 설정되니 적당히 수정 후 아래의 --- 밑에 부터 마크다운 문법에 맞춰 글을 작성하시고 저장해주시면 됩니다.

글 작성시에 실시간으로 포스팅된 모습을 확인하고 싶을 땐 터미널에서 다음과 같은 명령어를 실행 후
``` bash
$ hexo server  #기본 명령어
$ hexo s       #축약형 명령어
```
웹 브라우저에서 `localhost:4000`으로 접속하시면 실시간으로 블로그를 확인할 수 있습니다.

포스트 작성 후 실제 서버에 올라갈 블로그 정적페이지를 생성해주어야 합니다.
터미널에서
``` bash
$ hexo generate   #기본 명령어
$ hexo g          #축약형 명령어
```
를 실행하면 `blog/public/` 디렉토리 아래에 설정된 테마가 적용되어 실제 서버에 올라갈 웹페이지가 생성됩니다.

## Github에 Deploy하기
포스트 작성후에 외부에서 볼 수 있도록 서버에 발행하여야 합니다. 여기서는 github를 활용한 발행에 대해서 다룹니다.

1. 우선 깃허브에 로그인하여 원하는 repository를 하나 생성해줍니다.
  ![깃허브 repository 생성](/assets/images/github-create-repo.png)

2. 다음엔 Github에 Deploy해줄 플러그인을 설치합니다.
  ``` bash
  $ npm install --save hexo-deployer-git
  ```

3. Hexo 설정파일인 `_config.yml`에 발행정보를 추가해줍니다.
  `_config.yml`을 열어 'URL' 부분과 'Deployment' 부분 두군데를 수정해줍니다.
  ![Hexo URL Info](/assets/images/hexo-config-yml-url.png) 'URL' 부분을 아래와 같이 수정해주고,
  ``` python
  # URL
  ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
  url: https://lattecom.github.io/blogtest  # https://깃허브계정명.github.io/저장소이름
  root: /blogtest/      #/저장소이름/
  permalink: :year/:month/:day/:title/
  permalink_defaults:
  ```
  ![Hexo Deployment Info](/assets/images/hexo-config-yml-deploy.png) 'Deployment' 부분을 다음과 같이 수정해줍니다
  ``` python
  # Deployment
  ## Docs: https://hexo.io/docs/deployment.html
  deploy:
    type: git   #우리는 github에 발행하기를 하고 있으므로 git으로 합니다.
    repo: https://github.com/Lattecom/blogtest.git  #1번에서 만든 repo의 주소를 넣어주세요.
    branch: gh-pages    #gh-pages 브랜치로 설정해 줍니다.
  ```

4. Github 서버에 파일 올리기.
  터미널에서 발행 명령어를 실행하여 서버(여기선 Github)에 블로그 파일을 올려줍니다.
  ``` bash
  $ hexo deployment   #기본 명령어
  $ hexo d            #축약형 명령어
  ```

5. Github Pages에 접속하여 블로그 확인.
  기본 주소로 `http://깃허브계정명.github.io/리포이름`으로 접속할 수 있습니다.
  위에서 예제로 만든 블로그는 https://lattecom.github.io/blogtest 가 주소가 됩니다.


이상으로 Github와 Hexo를 활용하여 블로그 발행하는 방법에 대해 알아보았습니다.

---

## 별첨. Github Pages에 대하여
기본적으로 Github에 있는 Repository는 'gh-pages' 브랜치에 올라간 웹파일들을 `http://깃허브계정명.github.io/리포이름` 과 같은 주소로 제공해줍니다.

그 중에 특별하게 Repository 이름을 `계정이름.github.io`로 만들면 해당 Repo의 'master' 브랜치에 올라간 웹파일들을 뒤에 추가적인 디렉토리 URL이 붙지 않고  `https://계정이름.github.io`로 제공되어 사용할 수 있습니다.
