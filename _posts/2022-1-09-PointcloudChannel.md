---
layout: post
title: Pointcloul Channel을 이용하여서 clustering에 활용
category: Machine Vision
tag: Machine Vision
---

KD TREE 나 clustering을 한 이후에 우리는 Rviz나 PCL VIEWER에 Visualzation이나, class id를 포인터에 저장하고 싶다.

std::vector를 이용해서 point idx별로 class id를 구분 할 수 도 있지만, 더 효율적으로 더 똑똑하게 하고자 우리는 sensor_msgs/Pointcloud의 데이터 타입을 이용하여서 할 수 있다.

이때 Pointcloud1경우 channel이라는 data에 대한 이름을 정하고, 거기다 class id도 줄 수 있따. 이를 이용하여서 clustering에 대한 정보를 저장하여서 이후 topic으로 쏠때 받는 사람이 clustering에 대한 id도 같이 받을 수 있다.

아래와 같은 참조를 통해서 구현하였다.

http://www.360doc.com/content/17/0505/17/41168298_651355475.shtml

https://zhuanlan.zhihu.com/p/145939299

https://blog.csdn.net/lovelyaiq/article/details/118530388

https://blog.csdn.net/xhtchina/article/details/119707553

http://t.zoukankan.com/conscience-remain-p-14288723.html

https://zhuanlan.zhihu.com/p/145939299
