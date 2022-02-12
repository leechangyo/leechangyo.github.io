---
layout: post
title: CMAKE 이후 생성된 LIB과 특정 cfg share folder에 보존하기
category: Programming
tag: Programming
---

CI/CD를 통해 패키지를 업데이트를 하면, 특정 share folder에 Lib(Object File)과 Config를 생성하게 하고 싶다. 그렇다면 CMake에 아래와 같이 원하는 Lib과 폴더를 추가하면 끝!


```

install(TARGETS {add_library_lib} DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(TARGETS {add_library_lib} DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(TARGETS {add_library_lib} DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(TARGETS {executable_exe} DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(TARGETS {add_library_lib} DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(DIRECTORY launch/

DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch

PATTERN ".svn" EXCLUDE)

# install(DIRECTORY include/

# DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/include

# PATTERN ".svn" EXCLUDE)

install(DIRECTORY cfg/

DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg

PATTERN ".svn" EXCLUDE)

install(DIRECTORY scripts/

DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/scripts)
```
