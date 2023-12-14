---
title: "Advent of Supercomputing: Day 19"
date: 2023-19-19
publishDate: 2023-19-14T08:00:00Z
author: ["Austin Garcia"]
draft: false
---

# `Pushd and Popd`

## Mastering the Directory Stack
Tired of using `cd` and `ls` to switch back and forth between a few directories? You might have used `pushd` and `popd` to help you.

### The Basics
- `pushd`: Pushes a directory onto the stack and navigates to it.
- `popd`: Pops the top directory off the stack and navigates to the new top.
- `dirs`: Echoes the current stack.

### Cycling Through the Stack
The stack isn't just for pushing and popping. You can also cycle through it, moving directories around to suit your workflow.

#### Moving to a Specific Stack Position
```bash
$ pushd +n
```
This command rotates the stack, bringing the nth directory to the top and switching to it. Here, `n` is the index in the stack (starting at 0).

#### Example
```bash
~$ dirs
~ ~/project ~/downloads
~$ pushd +2
~/downloads ~ ~/project
~/downloads$
```
This would bring `/home/user/downloads` to the top and switch to that directory.

#### Rotating the Stack
Using `pushd` with no arguments swaps the top two directories:

```bash
$ pushd
```

## Pro Tips
- `dirs -v` prints the stack with index numbers, making it easier to use `pushd +n`.
- Combine `pushd`, `popd`, and `dirs` in scripts to handle directory changes more elegantly.
- Remember that `pushd` and `popd` affect the stack globally in your shell session. Changes in one part of a script can affect another unless managed carefully.

## Conclusion
Understanding how to manipulate the directory stack with `pushd` and `popd` allows for sophisticated and efficient navigation in the terminal. By mastering these commands, you gain a powerful tool to quickly access a wide range of directories, making you more effective in your command-line endeavors. Just be cautious not to lose your way in the stack — it's easy to push too much onto your plate, so to speak!
