---
layout: post
title: Realsense GhostPointCloud 제거 하기(Realsense Parameter 세팅 방법)
category: 통신
tag: 통신
---

Stereo 매칭으로 Depth Image를 추정을 하는데 우리가 Dense하게 하고 싶으면 Stereo matching(epiploar contraint(epiploar pole and line)으로 right 이미지에 있는 픽셀을 left이미지의 픽셀을 통해 매칭으로 depth를 추정)의 매칭에 대한 정확도를 낮추면 픽셀간에 매칭이 많이 이루워져 덴스한 뎁스데이터를 얻을 수 있다.

허나 환경 brightness가 계속적으로 변화하는 등 조명에 대한 민감도가 높아지기 때문에 Ghost Point가 생길 수 있다.(잘못된 매칭으로 통해 생기는 depth outlier)

이는 조명이 계속 바뀌는 공간에서 취약점이다.

이에 Realsense에서 제공을 하는 SDK를 설치하여서 Stereo Matching에 대한 Parameter를 수정을 통해 Ghost Point를 제거할 수 있다.

1. Command Reaselsene-Viewer in terminal

2. Advance Mode Actiavte

3. Second peak 조절

4. (Option) Manipulate another parameters

5. Save(icon placed beside on name of sensor in tab)

6. save parameter file can be used in realsense launch file.(will add in future how to do that)

7. (Option) Also we can load the saved file to Realsense Viewer to check whether parameter propely set or not



**이런식으로 파라미터 조정이 가능하며, 꼭 ros에서 제공하는 파라미터로 하드웨어 싱크나 파라미터를 맞추는 것이 아닌 리얼센스로부터 파라미터를 수정/저장한다음 roslaunch에 적용을 하여서 extric이든 instric이든 하드웨어 싱크 조절 및 ghost parameter도 없앨 수 있다.**
