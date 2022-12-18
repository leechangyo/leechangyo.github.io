---
layout: post
title: understanding cmake by experience and error(0)
category: Programming
tag: Programming
---

# CMAKE 에서 역할

well written about CMAKE!

[https://preshing.com/20170522/learn-cmakes-scripting-language-in-15-minutes/#:~:text=A CMake script defines targets,were defined in a subdirectory](https://preshing.com/20170522/learn-cmakes-scripting-language-in-15-minutes/#:~:text=A%20CMake%20script%20defines%20targets,were%20defined%20in%20a%20subdirectory).

Sub_Directory: cmake inside of Cmakelist of third party library.

# Bag of words library

add_subdirectory(third_party/DBoW2)

# SIFT feature library

add_subdirectory(third_party/ezSIFT/platforms/desktop)

Set Role : To define a variable inside a script, use the `[set](https://cmake.org/cmake/help/latest/command/set.html)` command

set( DBoW2_INCLUDE_DIRS  "${CMAKE_CURRENT_SOURCE_DIR}/third_party/DBoW2/include/") # "/usr/local/include")

set( DBoW2_LIBS         DBoW2) # "/usr/local/lib/libDBoW2.so")

set( EZSIFT_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/third_party/ezSIFT/include/" )

set( EZSIFT_LIBS  ezsift) # "${CMAKE_CURRENT_SOURCE_DIR}/third_party/ezSIFT/platforms/desktop/build/lib/libezsift.so")

라이브러리나, 인클루드를 사용자에 의해 지정을 해줄 수 있다.

Sub Directoy를 통해 Library의 lib을 읽는다.

beware, catkin_make read once all cmakelist inside of ws, and if read same library of add_subdirectory in many package, it will cause compile error. add_subdirectoy once is enough.

beware when add_subdirecoty written once on some pacakge,   using “Set” to get include and lib of library that you need to specific package.

# Catkin_Make, Beware

when you do catkin_make on some workspace, you might get some error about conflicting library name(duplicated when it read all the cmakelist in workspace.

- please check  packages that add_subidrectory has been written another package.
- if that so, only remain one, please disable the add_subidrecoty line.
