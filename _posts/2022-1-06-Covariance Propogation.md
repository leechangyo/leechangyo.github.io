---
layout: post
title: Covariance Propagation meaning
category: calculus
tag: calculus
---

통계학에서 불확실성의 전파 ( propagation of error )

나 오차 전파 ( propagation of  error ) 는

변수들의 함수관계에서 오는 불확실성에 영향을 주는

변수의 불확실성 효과 또는 오차 효과를 말한다.

변수가 실험실 측정의 값이라면

측정 도구의 정교함과 같은 측정 한계로 인해

변수는 불확실성을 가지고 이는 함수관계에 있는 다른 변수 결합에

전파되어진다.

오차는 실험을 하게되면 필연적으로 발생하는 것이다.
그런데 이 오차를 가지고 있는 값들을 서로 더하거나 곱하는 연산을 해야하는 필요도 상당히 빈번하다.

많은 경우, 이런 의문이 발생한다.
예를 들어 우리가 x라는 값을 측정했을 때 이 값이

x라는 평균값을 가지고 δx라는 오차를 가지고 있다는 것을 알게 되었다고 하자.

뭐, 숫자로 예를 들어서, x가 10이고 δx가 1라고 해보자.

그렇다면, x = x ± δx = 10 ± 1일 것이다.

여기서 의문은...
그렇다면, x2의 오차나 x-1의 오차는 어떻게 계산하며...

또 다른 오차를 가진 값인 y = y ± δy와의 연산인...
x+y의 총 오차는 어떻게 평가하는 지에 대한 것이다.

이에 대해 계산하는 것을 연산에 따라서 오차가 전달되어 간다는 의미에서...
오차의 전파(error propagation)라고 한다.

그렇다면 Covariance Propagation은 무엇일까.

과거의 Covariance(오차가 존재한다는 가정하에 )값들을 이후 Covariance에 전파를 해서 Covariance의 Variance가 점점 커지게 될 것이다. 이에 대한 Covariance의 Variance가 점점 커지는 것을 보정해야 할 필요가 있다.(Kalman Filter 등등 사용)
