---
layout: post
title: Smart Stick for the blind
category: Sensor fusion
tag: Sensor fusion
---

이번 글에서는 대학교 3학년때 프로젝트를 하면서 개발한 Smart Stick 소개드리려 합니다.

## Every thing is within Smart Devices

<a href="https://imgur.com/kI8ONuJ"><img src="https://i.imgur.com/kI8ONuJ.png" width="700px" title="source: imgur.com" /></a>

**2015년도** 친구들과 함께 캡스톤 디자인 브레인스토밍을 하면서 앨런 머스크 말 처럼 사람은 점점 사이보그로 진화하고 있다고 생각했다. 사람과 핸드폰만 있으면 무엇이든 정보와 지식을 얻을 수 있고 해결 할 수 있다. 하지만 이 스마트 시대에 있어 아직 이런 편리를 느끼시지 못하는 분들이 있다.

<a href="https://imgur.com/cBuxAIX"><img src="https://i.imgur.com/cBuxAIX.png" width="700px" title="source: imgur.com" /></a>

우리는 이런 통계를 보고 우리가 주어진 시간 안에 만들 수 있는 발명품이 무엇이 있을지 고민하였고, 시각장애를 가지고 계신 분들을 위한 스마트스틱을 만들자는 결론이 나왔다.

## FIRST OUR DESIGN & GOAL

<a href="https://imgur.com/aPUQXfa"><img src="https://i.imgur.com/aPUQXfa.png" width="700px" title="source: imgur.com" /></a>

우리는 인터넷 조사 결과 시각 장애를 가지신 분들은 내리막 길이 어렵다는 것을 알게 되었다. 이에 적외선 센서를 이용하여 휴대하기 편하게 손전등이 같이 개발하여 일정 각도와 안전 거리에 벗어나게 되면 계단이라고 인식하는 알고리즘을 만들기로 하였다.

<a href="https://imgur.com/YgUSCTd"><img src="https://i.imgur.com/YgUSCTd.png" width="700px" title="source: imgur.com" /></a>

하지만 법규상 시각장애를 가지고 계신 분들은 안전상의 목적으로 하얀색 지팡이를 지니고 다녀야 한다라는 법적 조항이 있다. 이에 우리는 흰지팡이에 **아두이노**를 이용하여 **적외선 센서들과 초음파 센서, 자이로센서** 등을 부착하여 스마트 지팡이를 만들기로 하였다.

<a href="https://imgur.com/ZzY5Vef"><img src="https://i.imgur.com/ZzY5Vef.png" width="700px" title="source: imgur.com" /></a>

## Introduction of EYE-Devices's functions

<a href="https://imgur.com/qxOe72Y"><img src="https://i.imgur.com/qxOe72Y.png" width="700px" title="source: imgur.com" /></a>

**첫번째** 기능으로 초음파 센서를 사용하여 **3미터** 앞에 있는 장애물을 탐지 할 수 있게 프로그램 하였다.

 <a href="https://imgur.com/PrgSwyb"><img src="https://i.imgur.com/PrgSwyb.png" width="700px" title="source: imgur.com" /></a>

 **두번째** 기능으로 자이로 센서와 적외선 센서를 이용하여 **안전거리 범위에서 갑자기 적외선 센서의 측정거리가 높아지거나 각도가 커지면 부저 센서를 이용해 알람을 울리는 기능을 만들었다.**

  <a href="https://imgur.com/NhzGnzJ"><img src="https://i.imgur.com/NhzGnzJ.png" width="700px" title="source: imgur.com" /></a>

**세번째**로 집안에서 지팡이를 찾지 못하거나 잃어버렸을 경우 핸드폰과 연동 bluetooth를 통해 부저가 울리도록 설계하였다.

  <a href="https://imgur.com/fRAaNDj"><img src="https://i.imgur.com/fRAaNDj.png" width="700px" title="source: imgur.com" /></a>

**마지막**으로 지팡이 끝에 전선을 연결하여 물 웅덩이에 닿았을 경우 부저가 울려 사용자가 웅덩이를 피해갈 수 있게 설계하였다.

## Future outlook & improvement
  <a href="https://imgur.com/wMzHyo3"><img src="https://i.imgur.com/wMzHyo3.png" width="700px" title="source: imgur.com" /></a>

안타깝게도 약 4년전에 만든 작품이라 저장해 놓은 작동하는 동영상을 백업하지 못하여 보여드릴 수 없는점이 아쉽다. 우리는 만든 작품들을 다산관 대강당에서 발표를 하였고 좋은 Demostration과 함께 좋은 점수를 받았던 것을 기억한다. 4년전에 같이 고생한 **오세홍, 봉석이형, 성호** 에게 감사하다. 이것으로 포스터를 마치겠습니다.
