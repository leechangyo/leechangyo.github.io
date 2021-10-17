---
layout: post
title: [C++, ROS] undefined reference
category: Programming
tag: Programming
---

```
오늘은 undefined reference to 에러에 대해 알아보겠습니다. 결론적으로 이 에러는 컴파일 시, 헤더 파일에 선언은 되어있으나, 소스 파일에 정의가 안되어 있다는 의미입니다. 함수는 보통 헤더 파일에 선언이 되고, 소스 파일에 정의를 하여서 많이 사용합니다

즉, 오타가 많더라구요. 그 외의 경우는 헤더파일이 빠져있는 경우도 많았습니다.

```

한 달동안 C 코드를 안하다 보니, 코딩을하는데 많은 부분을 까먹었다.

특히, 코딩중 undefined reference가 많이 나왔는데 주로 일어나는 이유가

1. header 파일을 cpp에 지정 하지 않았다.
2. CMAKE에 find package, target library, execute, add library
3. package.xml
4. 불러오는 class에 Constructor(Initialization)만 하고 destructor를 안해주면, 위와 같은 undefined reference문제가 나온다.
5. call 하는 실제 함수가 없다
6. cpp파일에서 c파일에서 구현하는 함수를 call 할때 발생(해결 방법 https://simmmmmk.tistory.com/19)
7. target_link_libraries 항목을 찾아서 해당 라이브러리 이름을 추가해주면 되겠습니다



즉 확인

- 메소드를 구현했는가? .h에 선언만 해 놓고 .cpp에 구현은 아직 안 한게 아닌가?

- .cpp에 scope를 잘 지정했는가? void ClassName::method(){}이렇게 namespace를 잘 지정해줬는가?

- 1. 생성자 구현을 헤더 파일안에서 inline으로 해주거나, 2. g++로 컴파일할때 링크를 건드리거나.
