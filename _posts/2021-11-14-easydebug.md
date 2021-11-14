---
layout: post
title: 모듈을 때와서 쉽게 Debug 혹은 테스트 하는 방법
category: Programming
tag: Programming
---

```c++
// g++ eigen_test.cpp -o eigen_test -I /usr/include/eigen3/

#include <iostream>
#include <Eigen/Dense>

using namespace std;
using Eigen::MatrixXd;

int main()
{
    MatrixXd m = MatrixXd::::Random()
}

```

위와 같이 모듈을 따와서 해본다.
