---
title: "Advent of Supercomputing: Day 13"
date: 2023-12-13
publishDate: 2023-12-13:00:00Z
author: ["org"]
draft: true
---

# `ncdu`, `diskonaut`, `dust`

These commands are usually better (more interactive) tools to view disk usage than the `du` command itself. And sure, maybe using `balboa` or other GUI disk usage utilities can display how your disk is used just fine, but unlike them the servers we remote into usually don't have that option. (It's also just easier)

`ncdu` is literally `NCurses du`. It's simple and allows you to get around your system pretty easy while viewing file size. In order not to be repetitve you can read more about it here:
https://voidlinux.org/news/2018/12/advent-ncdu.html


`diskonaut` is a similar tool, but will fill the entire screen with blocks denoting the size of directories and files. While less configurable than `ncdu` it still allows one to delete files and traverse quite quickly (yay vim movement). It's written in Rust and can be installed via most default package managers, but also `cargo` of course if it wasn't available for whatever reason.

![diskonaut-image](/post-media/advent-2-diskonaut.png)

> It doesn't seem to follow symbolic links though, so I think I'll keep using `ncdu` - Paco


`dust`
