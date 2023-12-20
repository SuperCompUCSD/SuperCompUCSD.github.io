---
title: "Advent of Supercomputing: Day 8"
date: 2023-12-08
publishDate: 2023-12-8:00:00Z
authors: ["paco", "khai"]
draft: false
---

# `ncdu`, `diskonaut`, `dust`

These commands are usually better (more interactive) tools to view disk usage than the `du` command itself. And sure, maybe using `balboa` or other GUI disk usage utilities can display how your disk is used just fine, but unlike them the servers we remote into usually don't have that option. (It's also just easier)

`ncdu` is literally `NCurses du`. It's simple and allows you to get around your system pretty easy while viewing file size. It's pretty straightforward and I originally learnt it from: [advent-of-void-day-12](https://voidlinux.org/news/2018/12/advent-ncdu.html)


**`diskonaut`** is a similar tool, but will fill the entire screen with blocks denoting the size of directories and files. While less configurable than `ncdu` it still allows one to delete files and traverse quite quickly (yay vim movement). It's written in Rust and can be installed via most default package managers, but also `cargo` of course if it wasn't available for whatever reason.

![diskonaut-image](/post-media/advent-2023-media/advent-8-diskonaut.png)

> It doesn't seem to follow symbolic links though, so I think I'll keep using `ncdu` - Paco


And **`dust`** is a reimplementation of `du` in rust with some nicer visual formatting
```
$ dust
 26G   ┌── .cache                                   │                                                   ███ │   4%
 31G   ├── persttemp                                │                                                   ███ │   5%
 55G   ├── Ongoing                                  │                                                 █████ │   8%
 25G   │           ┌── data                         │                                               ░░░░▓██ │   4%
 25G   │         ┌─┴ SC2Data                        │                                               ░░░░▓██ │   4%
 25G   │       ┌─┴ StarCraft II                     │                                               ░░░░▓██ │   4%
 26G   │     ┌─┴ Program Files (x86)                │                                               ░░░░███ │   4%
 27G   │   ┌─┴ drive_c                              │                                               ░░░░███ │   4%
 27G   │ ┌─┴ starcraft-ii                           │                                               ░░░░███ │   4%
 80G   ├─┴ Games                                    │                                               ███████ │  11%
 41G   │ ┌── Images                                 │                                           ░░░░░░░████ │   6%
 56G   │ ├── musica.qcow2                           │                                           ░░░░░░█████ │   8%
136G   ├─┴ VMs                                      │                                           ███████████ │  20%
 23G   │           ┌── tf                           │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓██ │   3%
 25G   │         ┌─┴ team fortress 2                │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓██ │   4%
 29G   │         │     ┌── win                      │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓███ │   4%
 29G   │         │   ┌─┴ target                     │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓███ │   4%
 29G   │         │ ┌─┴ resource                     │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓███ │   4%
 29G   │         ├─┴ GOD EATER 3                    │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓███ │   4%
 30G   │         │   ┌── csgo                       │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓███ │   4%
 33G   │         │ ┌─┴ game                         │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓███ │   5%
 38G   │         ├─┴ Counter-Strike Global Offensive│                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓███ │   5%
 24G   │         │   ┌── BASE.CPK                   │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓██ │   4%
 39G   │         │ ┌─┴ CPK                          │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓████ │   6%
 39G   │         ├─┴ P5R                            │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓████ │   6%
 41G   │         │     ┌── Win64                    │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓████ │   6%
 41G   │         │   ┌─┴ paks                       │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓████ │   6%
 48G   │         │ ┌─┴ r2                           │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓████ │   7%
 63G   │         ├─┴ Titanfall2                     │                             ▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓█████ │   9%
268G   │       ┌─┴ common                           │                             ▒▒▓▓█████████████████████ │  38%
287G   │     ┌─┴ steamapps                          │                             ▒▒███████████████████████ │  41%
290G   │   ┌─┴ Steam                                │                             ▒▒███████████████████████ │  42%
314G   │ ┌─┴ share                                  │                             █████████████████████████ │  45%
319G   ├─┴ .local                                   │                             █████████████████████████ │  46%
699G ┌─┴ .                                          │██████████████████████████████████████████████████████ │ 100%
```
