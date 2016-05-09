title: Tranquilpeak 테마에 웹 폰트 적용하기
date: 2016-05-08 20:42:04
categories:
- blog
- hexo
tags:
- hexo
- theme
- tranquilpeak
---
hexo blog 테마로 [Tranquilpeak](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak)을 사용 중 입니다. 깔끔한 디자인에 반응형으로 모바일까지 잘 지원합니다. 다른 테마들과 비교하여 가장 마음에 들었던 부분은 검색 부분입니다. 대부분의 테마들은 구글 도메인 검색으로 대체한 것이 마음에 안들었는데, Tranquilpeak 테마는 [swiftype](https://swiftype.com)을 활용하여 플러그인 형태로 쉽게 적용하게 만들어 둔 것이 마음에 들어 사용하고 있습니다.
<!-- more -->
오늘은 이 테마에 Font를 제가 원하는 웹폰트를 적용한 방법을 남기려 합니다.

우선 사용할 웹 폰트 파일을 css에 import시켜 주어야합니다. Tranquilpeak 테마에서 전체 css파일을 관리하는 `tranquilpeak/source/_css/tranquilpeak.scss`파일을 열어 가장 마지막에 다음과 같이 사용할 웹폰트 url을 추가해줍니다.

``` scss
@import
    url(http://fonts.googleapis.com/earlyaccess/notosanskr.css),
    url(http://fonts.googleapis.com/earlyaccess/nanumgothiccoding.css);
```
저는 본고딕-한글과 나눔고딕코딩을 추가했습니다.

이후엔 실제 폰트 적용 부분을 수정해보겠습니다.
`tranquilpeak/source/_css/utils/_variables.scss` 파일 윗 부분에 font-family를 위한 변수 정의 부분에 아래와 같이 사용할 웹폰트를 추가해줍니다.

``` scss
$noto-sans-kr:          'Noto Sans KR', sans-serif;
$nanum-gothic-coding:   'Nanum Gothic Coding', monospace;
```
저는 위에서 추가해준 본고딕을 `$noto-sans-kr`로, 나눔고딕코딩을 `$nanum-gothic-coding`으로 정의해주었습니다. 아래 이미지 처럼 추가해주시면 됩니다.

![web font variables](/assets/images/web-font-variables.png)

이후에 기본 폰트 설정 값을 `$open-sans-sans-serif`에서 아래와 같이 `$noto-sans-kr`로 변경해주었습니다.

![default font](/assets/images/default-font.png)

이후에 사이트 전체의 font를 바꾸어 줍니다. 아래와 같이 `$font-families`에 있는 값들을 수정해주면 됩니다.

``` scss
$font-families: (
   // base
   'headings':          $open-sans-sans-serif,
   // components
   'code':              $menlo,
   'caption':           $merriweather-serif,
   'image-gallery':     $open-sans,
   'post-header-cover': $merriweather-serif,
   'post-meta':         $open-sans-sans-serif,
   'post-content':      $merriweather-serif,
   'post-excerpt-link': $open-sans-sans-serif,
   'highlight':         $menlo,
   // layout
   'sidebar':           $open-sans-sans-serif
);
```

저는 아래 사진처럼 코드 및 하이라이트 부분엔 나눔고딕코딩을, 이외 모든 부분엔 본고딕을 적용했습니다.
![font families web](/assets/images/font-families-web.png)
