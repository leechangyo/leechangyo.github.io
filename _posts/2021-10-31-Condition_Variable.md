---
layout: post
title: Condition Variable
category: C++
tag: C++
---

https://jungwoong.tistory.com/92

즉 효율적으로 쓰레드 관리를 할 수 있다.

std::condition_variable를 사용해서

이는 공유 변수가 변경이 되면 condition variable을 통해 통지를 하게 되고,

condition variable을 가지고 있는 local {}은 통지가 올때까지는 쓰레드 중단, 사용 하지 않다가.

condtion variable에 의해 공유 변수 변경이 통지가 되었을때, 깨어나면서 쓰레드가 돈다.

즉, 쓰레드를 계속 돌리도록 방치하는 것이 아니라, condition variable을 통해서 공유 변수의 변경을 통해 쓰레드를 깨우고 잠재운다.
