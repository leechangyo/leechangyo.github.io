---
layout: post
title: ~ is private within this context"[C++, ROS]
category: ROS
tag: ROS
---

```
/home/chan/catkin_ws/src/height_map/src/Height_Mapping.cpp:13:32: error: ‘virtual height_map::HeightMAP::~HeightMAP()’ is private within this context

13 | HeightMAP hello(nodeHandle_);
```

모듈에 nodehandle을 여러 class에 사용할때 contructor와 destructor를 class Publich에 구조화 하지 않았다면, 특정 클래스에서 constructor와 destructor이 publich으로 선언이 되지않았는데, 다른 클래스에서 call이 된다면, 접근에 대한 에러로 이런 문제가 뜬다.

즉

```c++
class A{
public:
  A();
  ~A();
}
```
