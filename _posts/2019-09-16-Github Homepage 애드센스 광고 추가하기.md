---
layout: post
title: Github 홈페이지 구글 애드센스 광고 사이드바,게시물 추가하기
category: Github
tag: 광고 추가
---

이번 글에서는 github 홈페이지에 구글 애드센스를 추가해보겠습니다.(구글링을 하면 자료가 많지만 제 홈페이지에 적용하는데 좀 오래걸렸습니다.) 그럼 시작하겠습니다.

## 구글애드센스 가입

Github 홈페이지에 **광고**를 추가하시려면 우선 구글 애드센스[Google Adsense](https://www.google.com/adsense/start/#/?modal_active=none)에 가입을 해야합니다.

<a href="https://imgur.com/a71pI1W"><img src="https://i.imgur.com/a71pI1W.png" width="700px" title="source: imgur.com" /></a>

구글 매인홈페이지에 들어가 위에 빨간 버튼을 눌러 가입을 합니다.(VPN사용중이라 일본어로 나오네요). 가입을 했으면 사이트 설정에 들어가서 사이트 추가를 누르셔서 광고를 기재하고자 하는 사이트를 입력합니다. (저는 이미 등록을 한 상태이기 때문에 **준비**로 나옵니다.)

<a href="https://imgur.com/eajGZoZ"><img src="https://i.imgur.com/eajGZoZ.jpg" width="700px" title="source: imgur.com" /></a>

입력을 하면 애드센스에서 사이트 연결을 해주는 코드를 줍니다.

<a href="https://imgur.com/uUDXknH"><img src="https://i.imgur.com/uUDXknH.jpg" width="700px" title="source: imgur.com" /></a>

이 코드를 include -> head.html 에서 제일 윗 상단 <head> 다음에 넣어주시면 됩니다.

<a href="https://imgur.com/XzSfnG6"><img src="https://i.imgur.com/XzSfnG6.png" width="700px" title="source: imgur.com" /></a>

위 코드를 삽입을 완료가 되었으면, 구글애드센스에 돌아가 완료 버튼을 누루시면 됩니다. 다른 블로그나 인터넷에서 보며는 게시물이 적을경우 구글애드센스 승인 거절을 당한다고 하는데, 저는 게시물도 적지만 구글애드센스 승인완료하는데 있어서 보통 2~3일 걸리는것 같습니다. 승인 거절을 당해도 거절에 대한 답변이 있으니 부족한 부분 보완하여서 재도전 하시면 될것 같습니다.


## AbSense 광고 삽입

구글애드센스 광고 합격을 받았으면 아래와 같이 이메일을 받게 되십니다.

<a href="https://imgur.com/2NMHMw7"><img src="https://i.imgur.com/2NMHMw7.png" width="700px" title="source: imgur.com" /></a>

 여기서 Get Started 버튼을 눌러서 이제 광고 삽입 하는 방법에 대해 설명하겠습니다.

<a href="https://imgur.com/RJKbsEJ"><img src="https://i.imgur.com/RJKbsEJ.png" width="700px" title="source: imgur.com" /></a>

구글애드센스 화면에 들어가서 Ads -> Ad Units에 들어갑니다.(한글 버전이신 분들은 그림따라 따라 오시면 됩니다.) 들어가면 3가지에 광고 유형이 있습니다.

- Display
- In-feed
- in-article

여기서 우리는 광고의 기본 형태인 Display형을 선택하시면 됩니다.

<a href="https://imgur.com/5xSimax"><img src="https://i.imgur.com/5xSimax.png" width="700px" title="source: imgur.com" /></a>

유닛을 선택하시면 이와 같은 창이 뜹니다. 맨 위에는 광고 형태에 대한 이름, 그리고 오른쪽 상단에는 광고 형태에 대한 종류가 있습니다(Square, Horizontal, Vertical). 그 옆에는 광고 크기를 자동으로 맞출 것인지 크기를 커스터마이즈 할 것인지 정할 수 있습니다. 우선 horizontal에 저는 저장을 하겠습니다.

<a href="https://imgur.com/jTWOJ9u"><img src="https://i.imgur.com/jTWOJ9u.png" width="700px" title="source: imgur.com" /></a>

저장을 하면 이와 같이 코드와 함께 창이 뜹니다. 코드를 복사하고 이제 포스터나, 사이드바에 광고를 기재하는 방법을 해보겠습니다.

{% include advertisements_horizon.html %}

## 게시물에 광고 삽입

이제 게시물에 광고를 삽입을 하겠습니다. 홈페이지 관리하는 프로젝트의 폴더에 들어가 Include 폴더에 advertisement.html 파일을 만들고, 구글애드센스에서 광고를 복사한 코드를 붙여 넣습니다.

<a href="https://imgur.com/mtij7b0"><img src="https://i.imgur.com/mtij7b0.png" width="700px" title="source: imgur.com" /></a>

이와 같이 붙여 넣기를 한 후 _layouts -> Page.html 에다가 이와 같이 코드를 써주시면 각 게시물에 광고가 설정이 완료가 됩니다.

<a href="https://imgur.com/jHqd2mU"><img src="https://i.imgur.com/jHqd2mU.png" width="700px" title="source: imgur.com" /></a>

## 빈공간 오른쪽 사이드바에 광고 게시하기

빈공간 오른쪽 사이드 바에 광고를 게시는 포스터 게시보다 조금 복잡합니다. 먼저 _scss 폴더 -> Component 폴더 -> _sidebar.scss 파일에 들어갑니다. 들어가시고 맨 아래로 스크롤을 내린다음 다음과 같은 코드를 추가시킵니다.

```python
# 오른쪽 배너
.fixed-bottom-right{
  position: fixed; bottom:auto;top: 105px;left: 90%; margin-bottom: 0 auto;z-index: 900;
}
# 왼쪽 배너
.fixed-bottom-left{
  position: fixed; bottom:auto;top: 105px; margin-bottom: 0 auto;z-index: 900;
}
```
완료가 되면 이러한 형태로 될 것 입니다..
<a href="https://imgur.com/Jcwx9tS"><img src="https://i.imgur.com/Jcwx9tS.png" width="700px" title="source: imgur.com" /></a>

**_sidebar.scss**에 위에 코드를 추가가 완료되었으면, include 폴더 -> sidebar.html에 들어갑니다. 들어가시고 맨 아래로 스크롤을 내린다음 다음과 같은 코드를 추가 시켜주시면 됩니다.

```python
<div class="fixed-bottom-right">
  <!-- {% include advertisements_small.html %} -->
</div>
```
완료가 되면 이러한 형태로 될 것 입니다.
<a href="https://imgur.com/FsuWCd9"><img src="https://i.imgur.com/FsuWCd9.png" width="700px" title="source: imgur.com" /></a>

그리고 git commit -> push를 하시고 1분 뒤 홈페이지를 열으시면 오른쪽 빈 공간 사이드 배너에 광고가 나오고, 각 게시물에 광고가 나올 것 입니다.

이번 포스팅은 여기서 마치겠습니다. 감사합니다.
