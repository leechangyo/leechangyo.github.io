---
layout: post
title: CPU에 대한 간단한 지식(thread, limit performance)
category: 통신
tag: 통신
---

1. 대부분의 컴퓨터는 저전력으로 CPU가 돌고 있다.
만약 저전력 모드를 풀면, 전력을 더 많이 먹는 대신에, 클럭의 속도가 올라가게 됨으로 퍼퍼먼스를 높일 수 있다.(약 4,5배)
임베디드에서 무언가를 할때 위와 같은 방법을 사용하면 좋으나, 만약 USB케이블 같은게 물려있다면, BUS에 전력공급이 부족할 수 있으므로 연결성에 문제가 될 수 있다.

2. htop에서 보여주는 것은 core가 아니라 thread이다.
CPU안에 컨트롤 유닛과 , 프로세스가 있고 그안에 듀얼 코어 싱글코어가 있고, 코어 안에는 2개정도의 쓰레드가 있다.

코어 안에 뜨레드가 있는데 Logic을 담당하고 있다.


3. Multi Thread를 사용하여서 프로그램을 돌릴경우 CPU 점유율이 더 올라가게 된다.

4. RB5, Jetson Nano, Rapsberry, LattePanda

RB5 > Jetson Nano > LattePanda > Rapsberry

even every board has difference processor and kernel.

Jetson Nano More about GPU performance side.

Rapsberry just toy


5. OS PID 자동 Scheduling을 해주기 때문에 No Confilct 가 된다.

6. python 3 vs python 2
Binary 64bit가 다르기 때문에 호환을 맞춰줘야 한다.

7. node 안에 처리가 느리거나, 쌓이게 된다면(buffer) CPU Resource 과부하의 원인이 되기도 한다./

8. USB 3.0 Type은 Ampere가 900ma 이다.

9. Because of USB Power Inssucient Problem if board can not afford, we need to industry hub get sufficinetly providing power.

10. LTE Connection
Router <- LTE <-> DNS Connection -> auto Detect -> LTE
