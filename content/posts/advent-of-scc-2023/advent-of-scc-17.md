---
title: "Advent of Supercomputing: Day 17"
date: 2023-12-17
publishDate: 2023-12-17T08:00:00Z
author: ["khai"]
draft: false
---

# `taskset`

`taskset` allows for you the user to query and set the CPU affinity of a given process -- and by extention what cpu or set of cpus on your system a process can run on. With this, there are 3 primary ways to use `taskset` to either act as a wrapper command to set the CPU affinity of a child process, to query the CPU affinity of a given process or to modify the cpu affinity of an already running process.

## Querying
```
$ taskset -p [pid]
```
This will return is the bitwise encoding for the cpus the kernel will move the process to given load or the cpu scheduler.
```
$ taskset -p 81996
pid 81996's current affinity mask: ffff
```
For example, given that the system I'm running on has 16 hardware threads. Expansing `ffff` out into its bitwise representation of `1111111111111111` where the leading bit corresponds to hardware thread id 15 and the least signifigant bit corresponds to hardware thread 0.

## Setting
```
$ taskset -pc [cpu-list] [pid]
```
So if given the same process we change its cpu affinity to only hardware threads 0 and 15 with
```
$ taskset -pc 0,15 81996
pid 81996's current affinity list: 0
pid 81996's new affinity list: 0,15
```
the bitwise encoding should be `8001` or `1000000000000001`
```
$ taskset -p 81996
pid 81996's current affinity mask: 8001
```
to confirm that we did indeed set the cpu id we can look at our htop output
```
 PID△ CPU USER       PRI  NI  VIRT   RES   SHR S  CPU% MEM%   TIME+  Command
81996  15 khaiv       20   0 33.6G  155M 96132 S   0.0  1.0  1:23.14 /opt/discord/Discord
```


## Dispatching
```
$ taskset --cpu-list [cpu-list] [command]
```
to use `taskset` to set the cpu affinity of a child process we use the above command. To confirm let use use
```
$ taskset --cpu-list 6 vkcube
```
in `htop` we find this is indeed true.

```
 PID△   CPU USER       PRI  NI  VIRT   RES   SHR S  CPU% MEM%   TIME+  Command
 161449   6 khaiv       20   0  621M   99M 74520 S   2.6  0.6  0:04.39 vkcube
```


