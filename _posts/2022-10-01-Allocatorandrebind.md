---
layout: post
title: Allocator 이해하기 그리고 rebind의미
category: C++
tag: C++
---

[simple allocation in c++](https://www.geeksforgeeks.org/stdallocator-in-cpp-with-examples/)


[what rebind do](https://stackoverflow.com/questions/14148756/what-does-template-rebind-do)  <- Custom List를 만들때 쓰인다. Allocate를 다시하는데 사용 즉, allocate하는 데이터가 단순 int, float이면 상관이 없는데, 노드 데이터 처럼 struct이나 class형식으로 list를 저장하면 옵젝트에서 저장하는 allocation의 수를 데이터 구조에 맞게 rebind해주느 작업이다.

[example](https://www.devoops.kr/tag/rebind)
