---
layout: post
title: GPS-IMU Sensor Fusion(Kalman Filter)
category: Sensor fusion
tag: Sensor fusion
---

우리가 차를 타다보면 핸드폰으로부터 GPS정보가 UTM-K좌표로 변환이 되어서 지도상의 우리의 위치를 알려주고, 속도도 알려주는데 이는 무슨 방법을 쓴걸까?

바로 Kalman Filter를 이용하여서 우리의 위치와 속도값을 추정을 해주는 것이다.

GPS에서 제공하는 데이터 값은 x,y,z값

IMU에서 제공하는 데이터 값은 x,y,z, vx,vy,angular velocity

GPS의 데이터는 절대좌표에서 쏴주는 데이터 값이고, IMU에서 주는 데이터는 상대좌표에서 쏴주는 데이터 값이다.

상대좌표에서 데이터 값은 Drift나 센서 노이즈로 인하여 시간이 갈수록 불안전해 진다.(error Propagation)

절재좌표도 또한 가림이나 환경적 요인으로 인해서 데이터가 불안정해진다.(error Propagation)

센서에 대한 이런 문제를 보안하기 위해서 주로 Kalman Filter를 대중적으로 사용하게 된다.

Kalman Filter는 Prediction Model과 Update Model이 있는데,

주로 Prediction Model은 상대좌표(IMU)에서 얻어지는 데이터를 가지고 상태를 예측한다.(Predicted means, Predicted Covariance Matrix)

Predict 데이터를 통해, 절대좌표에서 측정된 데이턱 값을 업데이트 해준다.(Kalman Gain, Covariance Propagation with Kalman Gain, Measurement Model)

아래와 같다.

<a href="https://postimg.cc/XG33bLDm"><img src="https://i.postimg.cc/d1Z0GSRs/Kakao-Talk-Image-2022-01-09-13-02-57.jpg" width="700px" title="source: imgur.com" /><a>

여기서 C는 error Matrix이다.

선형적인 환경에서는 이 칼만필터가 잘 사용되지만 사실 현실세계는 비선형(앞으로만 가지 않고 여러 방향으로 가기도 하기 때문에)이기 때문에 비선형 모델을 사용하여서 많은 문제를 풀기도 한다.

Kalman Filter도 마찬가지로 비선형 방식인 Extended Kalman Filter가 있는데 간단히 Motion Model과 Measurement Model에 Talor Series expension를 통하여 Jacobian Matrix로 상태추정을 하는 것입니다.

<a href="https://postimg.cc/w3Y5CGcF"><img src="https://i.postimg.cc/ZYCfdk9k/Kakao-Talk-Image-2022-01-09-13-32-24.jpg" width="700px" title="source: imgur.com" /><a>


### Code Reference

https://github.com/search?p=1&q=imu+gps&type=Repositories

https://limitsinx.tistory.com/78
