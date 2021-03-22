---
layout: post
title: 리눅스에서 bashrc file, Shell, 커맨드 Source, Export의미
category: Programming
tag: Programming
---

### Bashrc file

**Bashrc file** is a script file that's executed when a user logs in. the file itself contains a series of configuration for the terminal session. this includes setting up or enabling: coloring, completion, shell history, command aliases, and more. it is a hidden file and simple is command won't show the file.s

~/.bashrc는 alias와 bash가 수행될 때 실행되는 함수를 제어하는 지역적인 시스템 설정 관련 파일이다. 이들 alias와 함수들은 오직 그 사용자에게만 한정되며, 그 이외의 다른 사람에게 영향을 미치지 않는다.

### Shell

the **shell** is an interactive interace that allows users to execute other commands and utilites in linux and other unix-based operating systems. when you login to the operating system, the standard shell is displayed and allow you perform common operations such as copy files or restart the system.


- on most linux systems a program called **bash** (which stands for bourne Again shell, an enhanced version of the original Unix shell program, sh) acts as the shell program.


### Source

﻿**Source** is a shell built-in command which is used to read and execute the content of file(generally set of commands), passed as an argument in the current shell script. the Command after taking the content of the specified files passed it to the TCL interpreter as text script which then gets executed

- TCL interpreter is a high-level, general-purpose, interpreted, dynamic programming language. it is available for many operating system, allowing tcl code to run on a wide variety of systems.

**source** 명령어는 스크립트 파일을 수정한 후에 수정된 값을 바로 적용하기 위해 사용하는 명령어 이다. 예를들어 etc/bashrc 파일을 수정 후 저장하여도 수정한 내용이 바로 적용 되지 않는다. **source** 명령어를 통해 적용할수 있다.

### Export

the **export** command is a built-in utility of Linux Bash shell. it is used to ensure the environment variables and functions to be passed to child processes. the export command allow us to update the current session about the changes that have been made to the exported variables.

**export** 는 설정하고자 하는 변수를 환경변수로 등록할 때 쓴다.
