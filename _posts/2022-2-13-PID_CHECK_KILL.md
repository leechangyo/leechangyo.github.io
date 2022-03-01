---
layout: post
title: PID Check 그리고 Kill（command & c++)
category: Programming
tag: Programming
---


### COMMAND

```
ps aux | grep {process id}
kill - {pid id}
```

[https://askubuntu.com/questions/180336/how-to-find-the-process-id-pid-of-a-running-terminal-program](https://askubuntu.com/questions/180336/how-to-find-the-process-id-pid-of-a-running-terminal-program)

[https://askubuntu.com/questions/797957/how-to-kill-a-daemon-process-in-linux](https://askubuntu.com/questions/797957/how-to-kill-a-daemon-process-in-linux)

[https://stackoverflow.com/questions/46479196/best-practice-pid-file-for-unix-daemon](https://stackoverflow.com/questions/46479196/best-practice-pid-file-for-unix-daemon)


### C++

```c++
#include <istream>
#include <sys/types.h>
#include <signal.h>

int main()
{
    pid_t pid=0;
    std::ifstream ifs("/var/run/httpd.pid", ios::in);

    ifs >> pid;
    if(pid)
        kill(pid, SIGTERM);

    return 0;
}
```

[SKILL PID IN C++](https://www.linuxquestions.org/questions/programming-9/killing-a-pid-in-c-94819/)
