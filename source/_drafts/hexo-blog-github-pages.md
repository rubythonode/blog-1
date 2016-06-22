title: Github pages와 Hexo를 이용하여 블로그 만들기
tags:
- hexo
- blog
- github
---

지난번 포스트([정적 블로그 프레임워크 Hexo](/2016/02/15/hexo-init/))에서 Hexo라는 정적블로그 프레임워크에 대해 간략하게 소개했습니다.

이번엔 hexo와 github의 부가기능인 github-pages를 이용하여 별도의 서버를 두지 않고 github의 repository에 블로그 파일을 올려 블로그를 게시하는 방법에 대해 이야기해보겠습니다.

## Hexo 설치
우선 아래와 같이 npm으로 Hexo 설치 후 blog 디렉토리에 기본파일들을 설치한다.

``` bash
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
```

위와 같이 설치후
