---
layout: post
title: Rvalue Reference(double address &&)가 무엇이고 왜 쓰는지 and "return *this" mean and "return reference" mean
category: C++
tag: C++
---

[RValue Reference definition](https://psychoria.tistory.com/54?category=556279)

[Move Semantic(Copy and Deconstuction을 반복하지 않고 효율적으로 데이터 처리하는 법), 여러 struct 및 class가진 데이터 구조를 vector나 어떤 컨테이너를 넣을때 효율적으로 할 때 사용(Move Construction)](https://psychoria.tistory.com/59?category=556279)

[Forwarding Problem과 Perfect Forwarding 여러 매개변수의 함수를 General하게 다 사용할 수 있도록 하는 곳에 사용(데이터 타입 캐스팅 가능)](https://psychoria.tistory.com/60?category=556279)

[RValue Reference와 LValue Reference를 받는 함수를 오버로딩 및 캐시팅할때 사용](https://psychoria.tistory.com/61?category=556279)


RValue Reference는 즉 temporally Hold Value and Move it, not copy.


["return reference" mean, copy가 아닌 LValue형식으로 global 변수가 있다고 가정을 할때 해당 메모리의 value를 바꿀때 사용 ](https://www.tutorialspoint.com/cplusplus/returning_values_by_reference.htm)

["return *this" mean, 바뀐 자기 자신의 class를 reference(LValue)타입 으로 자신 class메모리의 value를 변경 ](https://stackoverflow.com/questions/18162522/what-does-return-this-mean-in-c)
