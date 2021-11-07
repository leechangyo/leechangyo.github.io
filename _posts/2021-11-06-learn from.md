---
layout: post
title: 무엇을 배웠는가(1)
category: daily
tag: daily
---

1. RSLIDAR사용 할때 IP를 맞춰야 한다. 맞출 떄 Ubuntu -> Setting -> Network가서 연결 상태를 확인하고, ip를 확인하여서 rslidar sdk에 IP를 수정을 해주면 된다.

2. Rearsense camera로 마찬가지이다. 여기에 +로 쓸만한 launch는 rdgd.launch파일이 있는데 이것이 포인트클라우드로 변환을 시켜주는데, 엄청 편하다.

3. 아래 방법으로 Catkin_ignore을 통해서 워크스페이스 내에 필요없는 페키지 컴파일을 생략할 수 있다.

https://answers.ros.org/question/208180/how-to-use-catkin_ignore-file-correctly/

4. 켈리브레인션을 할때 손으로 대충 Transformation matrix를 맞춰줘야 하는데, 이때 자주 사용하고 확인 할 수 있는 방법은 static tf publisher(http://wiki.ros.org/tf#static_transform_publisher)이다. 혹은 내가 만든 icp_calibration_pkg를 이용하여서 할 수 있다.(base와 sensor link의 transformation matrix을 손으로 대충 만들고, 우리는 base 좌표계로 pointcloud를 보고 싶기때문에, 구해진 T 행렬을 inverse를 해서 pointcloud에 적용을 하면, base 좌표계로 pointcloud를 알 수 있다.)

5. 이와 같은 방식으로 projection을 할 수 있다. 즉 base 좌표 뿐만 아니라, 월드 좌표로까지 pointcloud를 프로젝션을 하여서 global map에도 표현을 할 수 있다. 즉 너무 쉽당. transformation matrix inverse를 잘해서 base link 나 work coordinate 표현, 혹은 sensor 좌표계로 tranformation matrix를 통핸 투영으로 표현을 하기도 쉽다.

6. 로켓챗, 레드마인, Gitlab

7. 파트장, 팀장님 리포터, 나머지 분들 Developer

8. MPS(megabytes per second) 초당 점유하는 메가바이트(램 점유율 확인)

9. Vector Maps both provide accurate metric information likewise Occupancy Grid Maps, and represent data as a graph that can be processed for path planning and maps merging as efficiently as with topological maps.(https://www.researchgate.net/publication/305804235_Vector_Maps_A_Lightweight_and_Accurate_Map_Format_for_Multi-robot_Systems)

10. 적정 로봇 센서 업데이트 및 프로세스 이후 주기는 30hz(imu 400hz, camera 60hz, lidar 5Hz/10Hz/20Hz)

11. 코어 600% 중에 30% 사용. 즉, 코어 6개(CPU) 중에 연산량을 얼마나 잡아 먹는지.(30%) 잡아먹음.

https://itblogpro.tistory.com/52

12. 우리가 개발한 로봇은 모두 완벽할 수없다. 다만 우리가 만든 테스트 셋 중에서는 완벽해야 된다.

13. 센서를 고르거나 테스트할 때 컴웨어 버전도 고려하자.

14. 데이터셋을 많이 뽑아서 자리에서 테스트 하자.

15. TOF -> CAN -> CAN_BRIDGE_  -> 제어기 -> 젯슨나노

16. R&R (Role and Responsibilities)

17. offine mapping
