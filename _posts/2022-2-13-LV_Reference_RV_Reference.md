---
layout: post
title: Deep Understanding of Reference LRValue and Pointer Relationship
category: C++
tag: C++
---

1. Concepts of left and right values

- The left value is a value that can be assigned on the left side of the assignment number; the left value must have an entity in memory;

- Right values are assigned to other variables on the right side of the assignment number; right values can be in memory or in CPU registers.

- When an object is used as a right value, its content (value) is used, and when it is used as a left value, its address is used.


2. citation

Reference is optimized by C++ grammar, and the essence of reference is realized by pointer. Reference to an alias equivalent to a variable.

References can change the pointer’s direction and the value pointed to by the pointer.
The basic rules cited are:

When declaring a reference, it must be initialized, and once bound, it cannot be bound to other objects; that is, the reference must be initialized, and the reference cannot be redefined;

All operations on references are equivalent to operations on the original object.

3. Explain it with Example.

(1) case
L Value는 R Value값을 읽는다.
Reference L Value는 R Value의 Address를 읽는다.

(2) case
L Value는 R Value값을 읽는다.
Reference R Value일 경우, L Value는 순수하게 Address를 읽는다.

(3) case

Pointer = 메모리공간의 일련번호(address) 그렇기 때문에 Pointer는 Address를 가르켜야 한다.

NOTE : 예제 3번에 마지막 cout에 b앞에 "*"를 넣어줘야 한다.

포인터는 어드레스를 읽지만, 표현할때(RValue로 사용할 때에는)에는 어드레이스에 있는 밸류를 표현하기 때문이다.

만약 RVALUE로 사용할때 앞에 "*" 붙여주지 않는다면, pointer의 address로 표현이 될것이다

<a href="https://postimg.cc/VdBPhQw8"><img src="https://i.postimg.cc/MKgZvqrv/Screen-Shot-2022-02-12-at-2-48-34-PM.png" width="700px" title="source: imgur.com" /><a>


```c++
void text(int& j)
{
  cout << j << endl;
}

void text(int* j)
{
  cout << *j << endl;
}

// those are samething
int i = 5;
test(i)


int i = 5;
test(&i)

```

### REFERENCE

[google](https://www.google.com/search?q=pointer+to+reference+c%2B%2B+mean&client=ubuntu&hs=BvU&channel=fs&sxsrf=APq-WBsUl_wR18xXrMqdVupv_soqDxD7mQ:1643852273939&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjrmef2suL1AhWDHKYKHcWtA-kQ_AUoAXoECAEQAw&biw=1608&bih=938&dpr=1#imgrc=53Pcz1UiEQ9-kM)


[difference between pointer and reference](https://www.educative.io/edpresso/differences-between-pointers-and-references-in-cpp)

[example of pointer reference](https://www.chriswirz.com/software/pointers-references-and-back-in-cpp)
