---
layout: post
title: 모듈을 때와서 쉽게 Debug 혹은 vscode debug 하는 방법
category: Programming
tag: Programming
---


### 1) 모듈 때와서 테스트 하기

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

### 2) VSCODE DEBUG모드

- cmake .. -DCMAKE_BUILD_TYPE=Debug

- vscode의 디버그 가서 node.js debug 아래와 같이 만들기

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "debug_image_database_management Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "/home/chan/Project/visual_database_management/bin/image_database_management_program",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        },



        {
            "name": "clang++ - Build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "lldb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C/C++: clang++ build active file",
            "miDebuggerPath": "/usr/bin/lldb-mi"
        }
    ]
}
```


#### VSCODE DEBUG 팁

**Debug actions#**

- Continue / Pause F5.
- Step Over F10.
- Step Into F11.
- Step Out Shift+F11.
- Restart Ctrl+Shift+F5.
- Stop Shift+F5.


### OPTIMIZE MODE

cmake .. -DCMAKE_BUILD_TYPE=Release
