---
layout: post
title: visual slam에 유용한 class & struct(1)
category: Programming
tag: Programming
---

CPU Utilization
https://peteris.rocks/blog/htop/

use htop

```
command : htop
```

**it visualize cpu和内存占率多少**

PID : Process ID(Every time a new process is started it is assigned an identification number (ID) which is called process ID or PID for short.)

PRI :
Priority(The priority (PRI) is the kernel-space priority that the Linux kernel is using. Priorities range from 0 to 139 and the range from 0 to 99 is real time and 100 to 139 for users.)

When you have more tasks to run than the number of available CPU cores, you somehow have to decide which tasks to run next and which ones to keep waiting. This is what the task scheduler is responsible for.

The scheduler in the Linux kernel is responsible for choosing which process on a run queue to pick next and it depends on the scheduler algorithm used in the kernel.

NI : Niceness (NI) is user-space priority to processes, ranging from -20 which is the highest priority to 19 which is the lowest priority. It can be confusing but you can think that a nice process yields to a less nice process. So the nicer a process is, the more it yields.

From what I've pieced together by reading StackOverflow and other sites, a niceness level increase by 1 should yield a 10% more CPU time to the process.

Memory usage - VIRT/RES/SHR/MEM
VIRT/VSZ - Virtual Image
The total amount of virtual memory used by the task. It includes all code, data and shared libraries plus pages that have been swapped out and pages that have been mapped but not used.
VIRT is virtual memory usage. It includes everything, including memory mapped files.

RES/RSS - Resident size
RES is resident memory usage i.e. what's currently in the physical memory.

While RES can be a better indicator of how much memory a process is using than VIRT, keep in mind that

    this does not include the swapped out memory
    some of the memory may be shared with other processes

If a process uses 1 GB of memory and it calls fork(), the result of forking will be two processes whose RES is both 1 GB but only 1 GB will actually be used since Linux uses copy-on-write.

SHR - Shared Mem size
The amount of shared memory used by a task. It simply reflects memory that could be potentially shared with other processes.

MEM% - Memory usage
A task's currently used share of available physical memory.
This is RES divided by the total RAM you have.
If RES is 400M and you have 8 gigabytes of RAM, MEM% will be 400/8192*100 = 4.88%.

TIME is the cumulative time the CPU (any CPU) spent running any thread in the process since it was started. So it can even go faster than real-time if you have several CPU cores and the process is multi-threaded.
https://unix.stackexchange.com/questions/121549/what-does-it-mean-exactly-when-a-processes-time-has-stopped-in-top
