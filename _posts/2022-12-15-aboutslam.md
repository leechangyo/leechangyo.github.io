---
layout: post
title: SLAM KR QUESTION AND ANSWER
category: SLAM
tag: SLAM
---

- GN is faster than LM
    - but easy fall in local minimal
    - if intiialization precisely and local minimal mostly none, then choose GN

- Superpoint 논문에서 매칭한거는 nn 매칭한 결과이고, superglue는 superpoint에서 추출된 descriptor를 입력받아 graph neural net에 기반하여 매칭하는거라고 보시면 됩니다. 즉, superpoint + superglue는 nearest neigbor matching(KD tree- euclidean distance) 을 사용하는것이 아닌(ML 방식), DL방식을 사용한것이다.

- 짐벌락은 3 파라미터를 가진 오일러 각을 최적화에 쓰지 않는 이유이다. 4파라미터인 쿼터니언보다 3파라미터가 계산량은 더 좋겠지만, 짐벌락이라는 현상떄문에 미분이 되지 않는다.
    - 최적화 할때 파라미터 값이 막 바뀌는데, Rotation Matix의 특징을 유지하면서 바꾸기는 어렵다. 쿼터니언은 그거에 비해 쉽다.

- Vins Mono에서 각속도 수식에 오메가 함수 안에 0값이 우하단에 들어가는데 이 경우 쿼터니언의 허수부가 앞에 있고 실수 부가 q = (q_x, q_y, q_z, q_w)

- p(t) = q(t)(p,1)q(t)^t 이런 쿼터니언 회전식 미분하면 각속도랑 관련된 식이 나오고 순허수 (w,0)꼴로 표현됐던걸로 기억합니다, 각속도는 유닛이 아니게 되고 각도랑도 상관이 없구요. 회전을 표현하는 유닛쿼터니온인지 각속도같은 다른 수치를 표현하는 쿼터니온인지 구분해서 해석하면 될거같습니다.

- 멀티 쓰레딩 잘하시는 분들은 생산 파이프라인 최적화 잘하실거 같다.

- 내츄럴 랜드마크 쓰기 어려운 이유, geometry 추정이 쉽지 ㅇ낳다. 절대적인 id와 상관관계를 찾기 어렵다
- ARM_NEON_x_x86 가속연산을 하다보면 보통 pc에서는 적용이 안되는데, 라이브러리를 통해 pc에서 바로 가속 연산 돌게해주는 라이브러리

- cross proudct은 면적의 의미를 갖고 있으니 벡터사이의 변화량을 crodd product으로 측정.
- 예를 들어 optical flow 변환한 좌표나, 포즈로 변환한 좌표나 완전히 같다면, 두베턱터에 방향이 같으니 cross product은 0이 된다.

It is "naive" in that the language and notations are those of ordinary informal mathematics, and in that **it does not deal with consistency or completeness of the axiom system**
. Likewise, an axiomatic set theory is not necessarily consistent: not necessarily free of paradoxes.

- VO과정 2D frame을 주어졌을떄, Feature extraction →feature matching(NN, RANSAC)→eight point algorithm → Essential Matrix → fundamental Matrix → SVD → Two Frame Pose Relatationship → 두카메라 포즈와 2d correspondence points들로 triangulation을 구하고 → pnp로 c2포즈 → Initialization이 끝났으면(SFM)
    - 위의 작업으로 triangualtion을 만들고 pnp로 포즈를 추정하는것을 반복한다.

- EKF LOCALIZATION(github , probabilisticrRrobotics
- depth의 단위는 m
- Lifelong Graph Learning : Feature 매칭을 temporally growing graph로 해석해서 그래프 러닝 기법을 적용한것
- Marker Tracking이나 Calibration은 최서 4개의 포인트가 필요하다. Normal Surface를 구하고 Rotation과 T값을 구한다.
- orientation은 말 그대로 카메라 현재 Rotation Matrix
- CheckBoard나 Tracker는 평면이라 기울여서 하면 안된다. Normal Surface에 대한 방향이 달라진다.
- **[실근(real root)](https://www.scienceall.com/%EC%8B%A4%EA%B7%BCreal-root/#:~:text=%EC%8B%A4%EA%B7%BC%EC%9D%B4%EB%9E%80%20%EB%B0%A9%EC%A0%95%EC%8B%9D%EC%9D%98%20%EA%B7%BC,%EC%9D%98%20%EC%8B%A4%EA%B7%BC%EB%8F%84%20%EA%B0%96%EC%A7%80%20%EC%95%8A%EB%8A%94%EB%8B%A4.) 즉 least Square Root는 실근을 구하는 것이다.**
- solvepnp: 3d point들과 correpnoding한 2d point를 가지고 pose 를 구하는것
    - 이에 corresponding이 안맞다면 잘못된 값이 나오게 될 수 있고
    - 3d 포인트가 평면위에 있는 점이라면 y축과 거의 가깝게 되기 때문에 pose를 구하기 어렵다.
    - pnp는 2d-3d 대응 관계가 주어졌을때 푸는것이기 되는데 위와 같은 상황이 된다면, 2d-2d가 되어 수치적으로 불안정해 진다.
    - y축 좌표가 거의 비슷해서 평면위 점이 되므로 수치적으로 (numerically) 2d위에 있는 점과 마찬가지가 된다.
- Save Trajectory를 보면 모든 프레임에서의 tracking pose를 save 해준다.
- 두이미지 특징점간 픽셀 차이를 parrallax
- normal equation 형태
- bayes 이론이 들어간 것들은  마르코프 Assumetion을 사용하여서 probability를 추정한다.
- 빛이 가득찬 환경에서는 로봇을 멈춰 빛이 지나가도록 하고, Tracking lost상태로 바꾼후 새로운 피쳐를 뽑아서 랜드마크에 대해 Relocalization 할 수 있도록 해야된다(빛이 카득찬 것은 버려야 한다.)
    - 혹은 imu나 휠 인코더로 그동안 모션 정보를 받아서 치환해주는 경우가 있다.
    - auto exposure를 빠르게 반응해준다면 해결할 수 도 있따.
- Postive Definite하다는 것은 minimum point가 있다는 것 정도로 Optimal Value가 있고, 매니폴드 타고 내려가면서 정답에 도달할 방법이 있다.
- BA에서 normal equation 행렬방식을 schur elimination로 mariginzation 한후 choleesky 혹은 LDLT Decompostion으로 풀릴 수 있으려면 Mariginzalization 결과 행렬이 Postive Definite Matrix여야 한다는 조건이 있어야 할 것 같은데 Marginzalization 결과가 항상 저 조건에 만족할 수 밖에 없는 이유는 Semi positive Definition,
    - [https://www.google.com/search?q=normal+equation+&sxsrf=APq-WBv7-GV2lrv7rHIwm2EO5HHaY3-_yw%3A1648094298936&ei=Wuw7YuDeOIyP2roPj_Sg2A8&ved=0ahUKEwjg8_nZ7d32AhWMh1YBHQ86CPsQ4dUDCA4&uact=5&oq=normal+equation+&gs_lcp=Cgxnd3Mtd2l6LXNlcnAQAzIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQ6BwgjEOoCECc6BAgjECc6BQgAEJECOgUILhCABDoHCC4Q1AIQQzoECC4QQzoECAAQQzoICC4QgAQQ1AI6BwgAEIAEEAo6BAgAEAo6CgguEMcBENEDEAo6CggAEIAEEIcCEBQ6CwguEIAEEMcBENEDOgcILhCABBAKOgoILhDHARCjAhBDOgcIIxCxAhAnSgQIQRgASgQIRhgAULVsWJSuAWD9swFoCXABeACAAbIBiAGUGZIBBDAuMjCYAQCgAQGwAQrAAQE&sclient=gws-wiz-serp](https://www.google.com/search?q=normal+equation+&sxsrf=APq-WBv7-GV2lrv7rHIwm2EO5HHaY3-_yw%3A1648094298936&ei=Wuw7YuDeOIyP2roPj_Sg2A8&ved=0ahUKEwjg8_nZ7d32AhWMh1YBHQ86CPsQ4dUDCA4&uact=5&oq=normal+equation+&gs_lcp=Cgxnd3Mtd2l6LXNlcnAQAzIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQ6BwgjEOoCECc6BAgjECc6BQgAEJECOgUILhCABDoHCC4Q1AIQQzoECC4QQzoECAAQQzoICC4QgAQQ1AI6BwgAEIAEEAo6BAgAEAo6CgguEMcBENEDEAo6CggAEIAEEIcCEBQ6CwguEIAEEMcBENEDOgcILhCABBAKOgoILhDHARCjAhBDOgcIIxCxAhAnSgQIQRgASgQIRhgAULVsWJSuAWD9swFoCXABeACAAbIBiAGUGZIBBDAuMjCYAQCgAQGwAQrAAQE&sclient=gws-wiz-serp)
    - [https://www.google.com/search?q=marginalization+probability&sxsrf=APq-WBv_UnWccW1h0aCFrH7K2JQyZ0tGuQ:1648094970620&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjQnp6a8N32AhUYDd4KHQrhA90Q_AUoAXoECAEQAw&biw=864&bih=1478&dpr=1.25](https://www.google.com/search?q=marginalization+probability&sxsrf=APq-WBv_UnWccW1h0aCFrH7K2JQyZ0tGuQ:1648094970620&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjQnp6a8N32AhUYDd4KHQrhA90Q_AUoAXoECAEQAw&biw=864&bih=1478&dpr=1.25)
- p3p + ransac 이 p6p보다 빠르다. 란삭은 운좋게 1번 운나쁘면 여러번이기 떄문에 steady하지 않다.
- Local Descritor 와 global Descritoptor의 차이
    - global은 이미지당 하나의 디스킵터이기 때문에 이미지와 이미지간에 얼마나 비슷한지 체크하는 것이고 lcoal descritor는 이미지간의 포인트 to 포인트 매칭으로 pnp한다던지 위치를 estimation하는데 쓰인다.
- Ceres solver 2.1나옴. 매니폴드 계산 지원 및 쿠다 지원
- rules.d 를 통해 usb를 구분할 수 있다.
