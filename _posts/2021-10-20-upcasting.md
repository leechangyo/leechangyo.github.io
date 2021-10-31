---
layout: post
title: upcasting, downcasting 과 virtual, override 관계
category: C++
tag: C++
---

상속 관계로 이뤄진 기반 클래스와 파생 클래스는 한 식구가 있다고 한다면 업캐스팅은 파생클래스에서 기반클래스로 캐스팅하는 것이다 즉

```c++
class cal
{
protected:
  int a,b,c;
public:
  void init(int new_a, int_b);
}

class add : public call{
public:
  void sum();
}

void main()
{
  add a;
  a.init(3,5);
}
```

파생클래스에서 기반클래스를 사용하게 되면 자동으로 업캐스팅을 된다.

다만 다운캐스팅은 자동적으로 되지 않기 떄문에 다른 방법을 써야 된다.

```c++
void main()
{
  cal* calptr;
  calptr = new add(3,5); // upcasting
  calptr->calcprn();

  add* addptr;
  addptr = (add*)calptr; // downcasting.
  add->calprn();
}
```

virtual과 override는 거의 같이 쓴다고 보면 된다.

virtual같은 경우 파생클래스와 기반클래스에 똑같은 이름의 함수가 있다면 기반클래스의 함수를 virtual로 픽스를 해주게 된다. 그리고, 파생에 이름이 같은 함수가 있을 경우 overide를 써준다.

즉 virtual은 기반클래스의 함수 구분용, override는 파생클래스 함수로 구분하는데 쓴다.

```c++
#include<iostream>
using namespace std;
class cal
{
protected:
  int a;
  int b;
public:
  cal();
  cal(int new_a, int new_b);
  virtual void ptr();
};

cal::cal()
{
  a = 0;
  b = 0;
}

cal::cal(int new_a, new_b)
{
  a = new_a;
  b = new_b;
}

void cal:prn()
{
  cout<<a;
}

class add : public cal
{
protected:
  int c;
public:
  add();
void ptr override ()

};

```

완전 가상함수 pure virtual function은 함수의 정의 없이 함수의 유형만을 기반 클래스에 제시해 놓는 것이다. 완전 가상함수는 가상함수처럼 멤버함수를 선언할 때 예약어 virtual을 선언문의 맨 앞에 붙이고 함수의 선언문 마지막 부분에 "=0"을 붙인다

```
virtual 반환형 함수영() = 0;
```

이렇게 파생 클래스, 기반클래스를 쓰게 된다면 한가집 꼭 해야되는 작업이 있다. 소멸자를 virtual로 해줘야 한다. 그래야 프로그램 종료가 될떄, 파생클래스의 소멸자도 불러오게 된다.

마약 virtual 을 안해주게 된다면

```
기반클래스 생성자
파생클래스 생성자
기반클래스 파생자
```

이렇게 콜이 된다. 파생클래스를 소멸하지 않았으므로, 메모리 릭같은 문제가 생길 수 있다.

그렇기 떄문에 기반클래스 파생클래스를 쓸떄는 기반클래스 소멸자에다가 virtual을 넣어서 파생클래스 또한 소멸자를 콜을 할 수 있도록 한다.

```
기반클래스 생성자
파생클래스 생성자
파생클래스 파생자
기반클래스 파생자
```
