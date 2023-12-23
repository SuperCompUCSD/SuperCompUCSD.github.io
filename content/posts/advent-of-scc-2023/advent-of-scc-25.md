---
title: "Advent of Supercomputing: Day 25"
date: 2023-12-25
publishDate: 2023-12-25T08:00:00Z
author: ["matei"]
draft: false
---

# Too Long, Didn't Read the other blog posts

Christmas Day is finally here! I'm sure that, over the past few weeks, you might
have missed one or two blog posts of ours. I don't blame you, the holiday cheer
conquers all desire to do anything else, really.

That being said, we'll leave you with one final awesome tool you can use whenever
you want to find out something new when working in the terminal. Not only that,
but here is a TL;DR of pretty much all of our posts for Advent of Supercomputing:

> Find cool commands and run `tldr` on them.

Wait, so uhhh, what's `tldr`?

# Too Long, Didn't Read the Manual

Man pages on Linux are an awesome way to check the reaches and functionality of
a program. Run `man` on a lot of low-level commands, and you'll find detailed
argument descriptions, runtime clarifications, etc.

```sh
>>> man git
```

That being said, Man pages are _verbose_ and comprehensive. But you don't _want_
comprehensive, that's too long! You probably want to figure things out fast and
work out from there. Well, `tldr` excels here: rather than dumping the entire
30-page user manual, `tldr` will show you a small set of examples on how to use
a given command, some of which will surprise you enough that you might find a
certain command is better than you gave it credit for.

First, we'll install `tldr`:
```sh
brew install tldr
```

This works on Arch Linux, Ubuntu, you name it.

Anyway, as a great example, we'll refer to
[this awesome **xkcd** comic](https://xkcd.com/1168/) as a point of reference
for what we want to do. Look, the `tar` command is hard to remember for almost
anyone, myself included. Rather than refreshing my memory using the man page,
I can just:

```sh
>>> tldr --update
# --->8---
>>> tldr tar
tar

Archiving utility.
Often combined with a compression method, such as gzip or bzip2.
More information: <https://www.gnu.org/software/tar>.

- [c]reate an archive and write it to a [f]ile:
    tar cf path/to/target.tar path/to/file1 path/to/file2 ...

- [c]reate a g[z]ipped archive and write it to a [f]ile:
    tar czf path/to/target.tar.gz path/to/file1 path/to/file2 ...

- [c]reate a g[z]ipped archive from a directory using relative paths:
    tar czf path/to/target.tar.gz --directory=path/to/directory .

- E[x]tract a (compressed) archive [f]ile into the current directory [v]erbosely:
    tar xvf path/to/source.tar[.gz|.bz2|.xz]

- E[x]tract a (compressed) archive [f]ile into the target directory:
    tar xf path/to/source.tar[.gz|.bz2|.xz] --directory=path/to/directory

- [c]reate a compressed archive and write it to a [f]ile, using the file extension to [a]utomatically determine the compression program:
    tar caf path/to/target.tar.xz path/to/file1 path/to/file2 ...

- Lis[t] the contents of a tar [f]ile [v]erbosely:
    tar tvf path/to/source.tar

- E[x]tract files matching a pattern from an archive [f]ile:
    tar xf path/to/source.tar --wildcards "*.html"
```

This is a _ton_ of examples that cover 99% of my usage of tar. There are a
surprisingly large number of cheat sheets from `tldr`, including for
_any of the commands from our Advent of Supercomputing_!

For instance, `mtr`, `gnuplot`, `xev`, `ffmpeg`, `fuck`, `wormhole`, `rclone`,
`rr`, `tesseract`, `mosh`, `figlet` and many more of the commands we've covered
all have `tldr` pages!

If you ever feel like you want to explore, or just want to see what the limits
of your own commands are, chances are that `tldr` will show you something new.
And fast, too!

Because the faster you move, the more you can do. Or, even better, the quicker
you can take a minute and enjoy the world as it is, the happier you'll be.

So, without further ado, for one final TL;DR moment:

**TL;DR:** `tldr` can show you cool stuff with `tldr --list` and you can learn a
ton of stuff, just like these blog posts.

**Have a wonderful Christmas and winter holiday!**