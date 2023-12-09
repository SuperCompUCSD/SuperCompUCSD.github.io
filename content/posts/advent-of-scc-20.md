---
title: "Advent of Supercomputing: Day 20"
date: 2023-12-08
#date: 2023-12-20
#publishDate: 2023-12-20T08:00:00Z
author: ["khai"]
draft: false
---

# `mktemp` and `tmpfs`

The TLDR is `mktemp` will make an empty file or directory in `/tmp` and depending on if you have `/tmp` setup to mount as `tmpfs` files in `/tmp` will exist in system ram rather than disk. Irrespective of if you have this setup or not, on reboot all data in `/tmp` will be cleared

## `mktemp`

The defaut behaviour for `mktemp` is to create an file in `/tmp` named `tmp.<some random id>` adding the `-d` flag makes the file created be a directory rather than a file.
```
$ mktemp
/tmp/tmp.jF2RFYXPII
```
the script I use to enter a temporary directory to do some quick work in is below
```
#!/usr/bin/bash
prev_dir=$PWD
dir=$(mktemp -d)
cd "$dir"
PREV=$prev_dir bash
rm -rf "$dir"
```
so when I exit the nested bash session the directory is automatically cleared.


## `tmpfs`
Why use `tmpfs` instead of just making it write to disk? depending on your system the primary benefit is write speed if you're generating tons of data to test something and the secondary benefit is if you're using tmpfs enough you endup not using up write cycles on you ssd or harddrive and thus increasing the life of the ssd. 

In the following tests the ssd being written to is a Micron 2300 NVMe 1024GB with a rated sequential write speed of 2.7 GB/s write and the system being used has dual channel 8gb 4800 MT/s LPDDR5 memory. testing the systems aggregate memory speed using the `stream` benchmark we get a little over 5 GB/s of for our memory speed.
```
$ OMP_NUM_THREADS=6 ./stream_c.exe
-------------------------------------------------------------
STREAM version $Revision: 5.10 $
-------------------------------------------------------------
This system uses 8 bytes per array element.
-------------------------------------------------------------
Array size = 100000000 (elements), Offset = 0 (elements)
Memory per array = 762.9 MiB (= 0.7 GiB).
Total memory required = 2288.8 MiB (= 2.2 GiB).
Each kernel will be executed 10 times.
 The *best* time for each kernel (excluding the first iteration)
 will be used to compute the reported bandwidth.
-------------------------------------------------------------
Number of Threads requested = 6
Number of Threads counted = 6
-------------------------------------------------------------
Your clock granularity/precision appears to be 1 microseconds.
Each test below will take on the order of 33231 microseconds.
   (= 33231 clock ticks)
Increase the size of the arrays if this shows that
you are not getting at least 20 clock ticks per test.
-------------------------------------------------------------
WARNING -- The above is only a rough guideline.
For best results, please be sure you know the
precision of your system timer.
-------------------------------------------------------------
Function    Best Rate MB/s  Avg time     Min time     Max time
Copy:           53443.8     0.030871     0.029938     0.035271
Scale:          49630.9     0.033272     0.032238     0.039681
Add:            53392.6     0.045858     0.044950     0.047513
Triad:          53000.2     0.046530     0.045283     0.050772
-------------------------------------------------------------
Solution Validates: avg error less than 1.000000e-13 on all three arrays
-------------------------------------------------------------
```


To show the benfit for write speed in `tmpfs` vs a ssd -- in this case it would be using the command
```
dd if=/dev/zero of=random.img count=1024 bs=10M
```
which will generate a file of 0s as fast as it can.

doing this in `$HOME` we get.

```
~ $ dd if=/dev/zero of=random.img count=1024 bs=10M
1024+0 records in
1024+0 records out
10737418240 bytes (11 GB, 10 GiB) copied, 4.93953 s, 2.2 GB/s
```

running this in a directory in `tmpfs` we get
```
tmp.oesyDZWAqI $ dd if=/dev/zero of=random.img count=1024 bs=10M
dd: error writing 'random.img': No space left on device
1024+0 records in
1024+0 records out
10737418240 bytes (11 GB, 10 GiB) copied, 2.10537 s, 5.1 GB/s
```

As we can see writing the file in `/tmp` which is mounted in system memory gets much faster write performance compared to writing to the system disk
