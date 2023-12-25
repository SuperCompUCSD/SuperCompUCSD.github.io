---
title: "Advent of Supercomputing: Day 25"
date: 2023-12-25
publishDate: 2023-12-25T08:00:00Z
author: ["matei", "paco"]
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

As a tip `tldr` can show you cool stuff with `tldr --list` and you can learn a
ton of stuff, just like these blog posts.

### Clients and other Options

(This section was written by Paco btw)

But... Matei, `tldr --list` doesn't work on my system. And when I saw someone use `tldr` the other day I was surprised that their client just output text&mdash; it's not like this on my system.

That is because different systems offer a different default `tldr` client. On Void Linux the default is written in Go, which offers fuzzy searching and interactions. It's very nice (except when the command turns my output invisible, so I have to enter `reset`), and I would recommend it over the plain output of pages of other clients. The Go client also gives [benchmarks](https://github.com/isacikgoz/tldr/wiki/Benchmarks) for the Go, Bash, Rust, C++, and Node clients btw.

![gif to go client screenplay was supposed to display :(. Just go to the repo linked instead: https://github.com/isacikgoz/tldr](https://raw.github.com/isacikgoz/tldr/master/img/screenplay.gif)

Digging into the parent, the `tldr-pages` repo, I see there are a lot of [clients](https://github.com/tldr-pages/tldr/wiki/tldr-pages-clients) written for `tldr-pages`, and... wait a minute... DuckDuckGo is on here? Apparently typing "tldr \<command\>" into the search engine will get your corresponding `tldr-pages` output on the right. What the duck, I didn't know that.

![Search "tldr man" on DuckDuckGo](/post-media/advent-2023-media/advent-25-duckduckgo-tldr.png)

There's more good stuff that I hadn't expected to find, so read the wiki if you're interested in perhaps creating your own entries or other: [wiki](https://github.com/tldr-pages/tldr/wiki)


Now as another mention the command `cheat` is a similar alternative to `tldr`, and we talked about `cheat` and `tldr` in our [terminal guide](/posts/austins-terminal-guide#tldr). And if you want more options the `tldr` git repo also has a mentions of few more alternatives of its own: devhints, `eg`, etc. Check out their [list](https://github.com/tldr-pages/tldr#similar-projects).


### Merry Christmas

The faster you move, the more you can do. Or, even better, the quicker
you can take a minute and enjoy the world as it is, the happier you'll be.

**Have a wonderful Christmas and winter holiday!**

