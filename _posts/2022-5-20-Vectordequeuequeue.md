---
layout: post
title: Vector vs Deque vs Queue
category: C++
tag: C++
---

1. Vector
- Vector get allocate address to each element in certain size or when push back automatcially in serial.
- that's why it works acces kind of below

```c++
std::vector<int> tmp(10,0);
tmp[4] = 4;
tmp.at(4) = 5;
```

- can access through address.

- therefore, when use need to erase, insert it Big O is O(n), because we need to pull element all back or front depending on using insert or erase function.

- recommend use it when you only need built once and read.

2. Dequeu
- it has stack and queue function
- but it is list type.(double list(node))
- they are not allocated element in memory in serial, that's why the above method is not works
- list type literally use iteration to access node(data).

```c++
std::deque<int> tmp(0,10);
auto itr = tmp.begin();
int tmp2 = *(itr + 4);
// get idx 4 element
```

- it is much more efficent than vector when you need to insert and erase often.
- but when you need to read often then not recommend use it.

3. quque

- it literally working quque.
- it is also list type container.
- everything is similiar dequeue.
