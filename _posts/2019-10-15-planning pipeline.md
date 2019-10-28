---
layout: post
title: planning pipeline 의미
category: ROS
tag: ROS
---

# Planning Pipeline
- Pipeline이란
  - 프로세서에서 성능을 높이기 위해서 명렁어 처리 과정으로 명령어 처리를 여러 단계로 나누어 단계볼로 동시에 수행하여 병렬화 시키는 것
- pipeline 동작 방식
  - 매 클록마다 여러 명령어를 중복된 단계 없이 실행킨다.

<a href="https://postimg.cc/hz8gZGSc"><img src="https://i.postimg.cc/bJWJJGWD/Capture.png" width="700px" title="source: imgur.com" /><a>

- 가장 효율인 pipeline 조건
  - 각 단계별 처리 시간이 일정 해야한다.
  - 각 명령어의 처리 단계는 균일 해야한다.
   - * 그러나 실제로는 처리 시간일 일정하지도 않고 처리 단계가 균등하지 않아서 문제가 발생한다.

- 헤저드의 종류(Hazard)
1) 구조 헤저드
  - 프로세서의 자원이 부족해서 발생한다.
  - 아래와 같이 명령어 2에서 EXE가 1클락에 안끝날 경우, 명령어 3에서 WB 수행할때 멈춤(stall) 이 발생한다.
  - 이를 구조 헤저드라한다.
  - 또한 명령어4 에서는 OF단계가 필요없는 명령어이므로 EXE를 수행하려고할시 명령어3에서 EXE가 수행되고 있어 이때 또한 Stall이 발생한다

<a href="https://postimg.cc/D8T5KwnR"><img src="https://i.postimg.cc/rFdHxDby/Capture.png" width="700px" title="source: imgur.com" /><a>

2) 데이터 헤저드
  - 이전 명령어의 결과를 기반으로 다음 명령이 수행될때 발생한다.

<a href="https://postimg.cc/VrP4LfXS"><img src="https://i.postimg.cc/mDtnjhDS/Capture.png" width="700px" title="source: imgur.com" /><a>

3) 컨트롤 헤저드
  - 프로그램의 의존성에 의해서 발생한다
  - 분기 분의 경우 해당 명령어 경과같이 나와야 다음 명령어가 수행되는데, 이때 해당 멸령어가 끝날때까지 다음 명령어가 멈춤이 발생한다. 이른 컨트롤 헤저드라고 한다.

<a href="https://postimg.cc/RNWtyrn2"><img src="https://i.postimg.cc/J4qQyM77/Capture.png" width="700px" title="source: imgur.com" /><a>

- 소프웨어에서 pipeline 기법 사용하기(매력적인 방법이다)
  - 만약 소프으웨어가 여러 단계로 독립적으로구현 될수 있다면, 다음 방식으로 pipeline으로 구현 할수 있다.
  - 1단계 : 각 단계를 Thread 구현
  - 2단계 : 각 Thread를 queue로 연결

<a href="https://postimg.cc/wyPqsrTs"><img src="https://i.postimg.cc/vT8cCRJX/Capture.png" width="700px" title="source: imgur.com" /><a>

# Reference
https://en.wikipedia.org/wiki/Pipeline_planning

https://doitnow-man.tistory.com/72
