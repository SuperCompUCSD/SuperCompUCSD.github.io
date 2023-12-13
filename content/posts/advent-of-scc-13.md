---
title: "Advent of Supercomputing: Day 13"
date: 2023-12-13
publishDate: 2023-12-13T08:00:00Z
author: ["ritoban"]
draft: false
---

# Zoxide

Tired of manually having to type out paths to `cd` to a directory? Or worse yet, clicking through them in a GUI file manager? Does `cd Docu<tab>/col<tab>/fa<tab>_22/<tab><tab>` feel like a familiar painful feeling?

[`zoxide`](https://github.com/ajeetdsouza/zoxide) to the rescue!

Zoxide is an excellent program that creates an alias `z` in your shell, that locally keeps a database of all the directories you visit, and then lets you automatically jump to the most common directory that matches closest to its argument. For example, to jump to the folder `Documents/college/fall_2023/cse_101`, I might just type `z 101`, and it'll take me straight there!

Here's an example of how quickly it's possible to move around using it:
[![asciicast](https://asciinema.org/a/NdY5vBf0EKEzVjevbSDYApYP0.svg)](https://asciinema.org/a/NdY5vBf0EKEzVjevbSDYApYP0)

To get started, head over to the [Zoxide Github page](https://github.com/ajeetdsouza/zoxide) and run the script corresponding to your shell (all major, and quite a few minor shells are supported). In particular, on bash, first

```
curl -sS https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | bash
```

and then add
```
eval "$(zoxide init bash)"
```

to the end of your `~/.bashrc`.

## Other similar projects
* [Autojump](https://github.com/wting/autojump) is a very similar idea, written in python
* [FZF](https://github.com/junegunn/fzf) supports somewhat similar functionality as a mapping, but with fuzzy finding for relevant directories

