---
title: "Advent of Supercomputing: Day 18"
date: 2023-12-18
publishDate: 2023-12-18T08:00:00Z
author: ["matei"]
draft: false
---

# I don't wanna commit `.DS_Store` anymore

As a (relatively recent) macOS user, I've worked on a few new repositories in
Git, and every single time I am annoyed when I accidentally commit `.DS_Store`
to a repo.

This happens in different ways often enough to make me want to automate this
away. And this isn't unique to just macOS's stupid files, by the way, it's a
problem with a lot of projects in general:

- No, Git, I (almost) always don't want to commit my `build/` directory from my Rust crate
- No, Git, I don't want to commit my `.env` file or my temporary JavaScript build files
- No, Git, I absolutely did not want to commit `hugo.exe` or `.hugo_build.lock`
  (has happened before with other blogs like this one)

Well, in those cases, there's a feature in Git to avoid that: `.gitignore`

# This file is cool

For every line in `.gitignore`, if any file or folder matches the naming (or
path), Git will just ignore them and pretend like they don't exist. So, you can
do stuff like:

```
.DS_Store
.hugo_build.locl
public/*           # Do not commit the "public" directory contents...
!public/image.ong  # ...but you can commit this one file only
```

Super simple, and super cool. However, there's enough boilerplate in Git ignore
files for common projects that you'll find that even GitHub lets you initialize
a project with one:

![A screencap of GitHub's repo creation screen, showing the "Add .gitignore" feature.](/post-media/advent-19-github.png)

Surely, there is a quick way to do that locally as well, right?

Of course there is. 😉

# Meet `git-ignore`

_Fun fact:_ Any executable named `git-*` can be called using `git *`, because
`git` automatically assumes such executables as extensions to the base
functionality. Coolio.

So, obviously, if you name an executable `git-ignore`, you can create a new
command. You can probably guess what this one does, I imagine.

If you can't, [you can read the README here](https://github.com/janniks/git-ignore),
but in essence, you get a command that lets you ping an online `.gitignore` generator
service and use it to generate a quick `.gitignore` template the exact way GitHub
does it, but from your command-line! No need to make GitHub initialize the repo
first. Plus, it's written in Rust, too!

Just call it in a new repo:
```sh
>>> git ignore
Loading ignore templates from GitHub...
Type to search for ignore templates
 - ENTER to select a template or accept selection
 - ESC to cancel at any time
Selected template: Emacs, VisualStudioCode
Search templates: macos
 > macOS
   ReactNative.macOS.stack
```

It's really that easy. There are alternatives, too, one of them being
[gibo](https://github.com/simonwhitaker/gibo), which I used for a while.

Anyway, hopefully you'll never accidentally end up committing weird files anymore.
And hey, if you do, [there are ways to nuke them from orbit I'm sure you'll love.](https://rtyley.github.io/bfg-repo-cleaner/).