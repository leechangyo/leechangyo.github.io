---
layout: post
title: Linux 중요한 명령어
category: programming
tag: Linux
---

## 1.SUDO
SuperUserDo 의미로 Permission이 제한된 작업을 할때 사용한다.
- $sudo su

## 2.ls(LIST)
Folder 나 보고 싶은 Directories & files를 보고 싶을 때 사용 한다.
- /home$ ls

## 3.CD(Changing Directory)
터미널에서 자주 쓰는 명령어로 디렉토리를 이동할 때 사용한다.
- cd some Directories
> example : /home$ cd url <- url 디렉토리로 이동한다.

- cd ..  (이전 디렉토리로 이동한다.)

## 4.mkdir(Make Directory)
폴더를 생성할 때 쓰인다.
- ~$ mkdir folderName

## 5.cp(Copy-and-paste)
터미널로 부터 폴더나 파일들을 복사하고 붙여 넣을 때 쓰인다.
- ~$ cp src des (src를 복사하여 des에다가 붙여 넣는다.)
> Permission이 있는 파일 들은 Sudo 명령어를 사용하여 cp 명령어를 사용한다.

## 6.rm(Remove)
삭제 명령어로 원하는 파일이나 폴더를 삭제할 수 있다.

- ~$ rm myfile.txt

## 7.apt-get
Advanced Packaging Tool(APT) Package 매니저로 소프트웨어를 설치하는데 사용한다.
- ~$ Sudo apt−get update

## 8.Grep
찾고자 하는 파일의 위치나 경로를 모를 때 사용 한다.
- ~$ grep user /etc/passwd

## 9.cat(concatenate)
cat명령어는 text 파일이나 코드들의 내용물을 볼때 터미널에서 사용된다.
- ~$ cat CMakeLists.txt
> root@server:/test/t1# cat hello.txt
> Hi, This is for testing only


## 10.poweroff
사용하고 있는 termnial 창을 종료 시킬때 사용한다.
- $ sudo poweroff

## 11.pwd
현재 디렉토리의 경로가 어떻게 되어있는지 print해서 터미널에 보여준다
- $ pwd

## 12.mv(move/rename)
파일을 다른 폴더로 옮기거나 이름을 바꿀때 사용 된다.
> root@server:/test# mv t1/* t2<br>
> root@server:/test# mv t2 test2<br>
> root@server:/test# ls<br>
> t1 test2

## 13. rmdir(Remove Directory)
말 그대로 디렉토리 제거하는데 쓰이는 명령어다.
> root@server:/test# ls <br>
> t1 test2 <br>
> root@server:/test# rmdir t1 <br>
> root@server:/test# ls <br>
> test2

## 14.touch(Create blank file)
touch 명령어는 파일을 만들어 내는데 쓰인다.
> root@server:/test/test2# touch empty.txt <br>
> root@server:/test/test2# ls <br>
> empty.txt hello.txt

## 15.free(check free ram)
컴퓨터 안에 사용할 수 있는 free ram을 체크한다.
> root@server:/test/test2# free <br>
> total  used  free shared buff/cache available <br>
> 2097152 598076 1417768 232288 81308 1424127 <br>

## 16.df(Disk Space)
컴퓨터 디스크 용량을 확인하는데 사용한다.
> root@server:/test# df -h
> Filesystem Size Used Avail Use% Mounted on <br>
> /dev/simfs 95G  2.3G  93G  3%   / <br>
> devtmpfs   1.0G 0     1.0G 0%   /dev <br>
> tmpfs      1.0G 0     1.0G 0%  /dev/shm <br>
> tmpfs      1.0G 99M   926M 10%  /run <br>
> tmpfs      5.0M  0    5.0M 0%  /run/lock <br>
> tmpfs      1.0G  0    1.0G 0%  /sys/fs/cgroup <br>
> none       1.0G  0    1.0G 0% /run/shm

## 17.chmod (change Permission)
파일의 Permission 상태를 쓰기가 가능하게 혹은 오직 읽기만 가능하게 변화 시켜준다.
> root@server:/test# chmod 755 test2 (755는 폴더 안에 있는 모든 파일의 permission을 사용할 수 있게 바꿔준다.) <br>
> root@server:/test# chmod + test.txt ("+"한가지 파일의 permission을 사용할 수 있게 바꿔준다.)

## 18.du(Directory usage)
디렉토리의 용량들을 확인할 수 있다.
>root@server:/test# du <br>
> 8  ./test2 <br>
> 12 .
