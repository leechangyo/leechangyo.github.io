---
layout: post
title: Realsense Driver and USB Connection Problem
category: 통신
tag: 통신
---

Realsense Noetic에서 드라이버가 끊켰다 연결되었다 하는 현상이 있다.

1. Realsense SDK문제
  - Realsense SDK가 불안정하여서 드라이버가 정상적으로 작동안하는 이슈가 있을 수 있다.
  - 혹은 ros launch file에 리얼센스 세팅이 잘 못 되어있을 경우가 있다.

2. Hardware 문제
  - USB케이블로 연결되어 있기 때문이 LAN 케이블보다 불안정함.
  - usb Hub의 usb bandwidth이 문제로 연결이 불안정할 수 있다.


해결 방안

1. Realsense SDK UPDATE
  - 2.48이전 버전은 Linux커널 최대 5.4까지만 지원해줌으로써 noetic같은 경우 5.8인데, 커널문제로 연결이 안될수 있다.
  - 이에 Realsense에서 제공하는 OS와 SDK호환 문제를 해결해주는 RS_BACKEND를 사용을 하면 문제를 해결할 수 있지만, CPU연산량이 배로 늘어나는 단점이 있다.
    - librealsense 깃헙에서 다운을 받는다
    - build를 만들고 cmake시 RS_BACKEND 모드로 cmake를 하면 된다.
    - make 빌드를 하면 끝

2. USB or DRIVER PID RESET
  - driver launch되는 pid 넘버를 제거하면 된다(블로그에 pid제거 방법 있음)
  - USB Connection RESET
  
[https://ikaros79.tistory.com/entry/Ubuntu에-Intel-RealSense-용-Firmware-Update하기](https://ikaros79.tistory.com/entry/Ubuntu%EC%97%90-Intel-RealSense-%EC%9A%A9-Firmware-Update%ED%95%98%EA%B8%B0)

[https://github.com/Marviel/TIL/blob/master/unix_tools/Reset_specific_USB_Device.md](https://github.com/Marviel/TIL/blob/master/unix_tools/Reset_specific_USB_Device.md)

[https://askubuntu.com/questions/645/how-do-you-reset-a-usb-device-from-the-command-line](https://askubuntu.com/questions/645/how-do-you-reset-a-usb-device-from-the-command-line)

[https://askubuntu.com/questions/1036341/unplug-and-plug-in-again-a-usb-device-in-the-terminal](https://askubuntu.com/questions/1036341/unplug-and-plug-in-again-a-usb-device-in-the-terminal)
