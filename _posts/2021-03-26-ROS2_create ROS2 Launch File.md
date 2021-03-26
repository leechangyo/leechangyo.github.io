---
layout: post
title: How to create a ROS2 Package for python
category: ROS2
tag: ROS2
---

COMMANDS TO USE:

- 만약 execute할 파일이 이미 있다면
- launch folder를 만들고
- execute.launch.py를 하여서 파이썬 스크립트로 런치파일을 한다
- 물론 이전 launch 형식으로도 execute할 노드들을 런치 할 수 있다.
- 마지막에다 CMakeLists에다 install launch files를 하여서 launch를 이용할 수록 한다.


py로하는 이유는, 노드 관리와 파라미터 관리가 편하다.



참고 자료:
https://www.youtube.com/watch?v=FfMWmWdPPAM&list=PLK0b4e05LnzYNBzqXNm9vFD9YXWp6honJ&index=4
