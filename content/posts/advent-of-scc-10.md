---
title: "Advent of Supercomputing: Day 10"
date: 2023-12-10
publishDate: 2023-12-10T08:00:00Z
author: ["matei"]
draft: false
---

# Git for lazy people

If you're a person who is at all adjacent to coding in any meaningful way,
whether as a software engineer, a PhD. student writing LaTeX documents, or a
supercomputing fanatic, you will _have_ to use Git at one point or another. In
fact, eventually, it will become like second nature, if not completely a part
of your shell script alias collection, with beautiful shortcuts such as `git
push --force-with-lease => gpfl`. Aaah, so readable.

That being said, I'm sure you already might have a couple of questions:

- How do I even remember all these aliases?
- What the hell is even `git push --force-with-lease`?

While [there exists an answer for the second
question](https://git-scm.com/docs/git-push#Documentation/git-push.txt---no-force-with-lease),
the first one is an issue as old as time. After all, a lot of people always
talk about the difficulty of handling Git as a whole, and frankly, sometimes I
can get annoyed at doing more complicated things myself.

So, naturally, I tried reaching for other ways to make my life easier when
using Git.

## I should just use a GUI, right?

To be honest, this is definitely an approach to make Git work better, but most
GUI's are either paid, proprietary, or not that functional. What's worse,
they're not even opinionated, they're massive hunks of enterprise software that
are hard to get a grip of.

I did however, look around further, only to find the coolest thing ever: a TUI
(terminal UI) for Git that works splendidly, called
[LazyGit](https://github.com/jesseduffield/lazygit).

## How does it work?

First, install it like any other software, on your distribution. Say, for
macOS:

```sh
brew install lazygit
```

Or Arch:

```sh
sudo pacman -S lazygit
```

Or Debian/Ubuntu/Raspbian:

```sh
sudo apt install -y lazygit
```

Now, `cd` into any Git repository and just open up `lazygit` by invoking it.
Certain editors (like Vim) have plugins where you can open LazyGit straight in
the editor in a floating window! However, we'll be using it from the terminal.

## What can I do with it?

For starters, LazyGit has a UI _very_ similar to another wondrous Git TUI that
I used to use, notably [Magit](https://magit.vc/) for Emacs. This thing is
still perfect, but unfortunately it's Emacs-only, which is not for everyone.
However, LazyGit looks and feels quite similar to Magit in its functionality.

For instance, below you'll find an example recording of me booting up LazyGit,
committing chunks of code using selective line staging, and then
pushing to GitHub:

![LazyGit demo](/images/LazyGit.gif)

To be frank, this is incredibly basic, but there are some really cool things
you can do with LazyGit easily, including but not limited to:

- swapping upstream and remote branches easily
- managing Git stashes line-by-line
- interactive rebasing with quick to remember keyboard shortcuts
- quickly undoing changes to Git repository
- Simplifying bisections

A lot of these are advanced Git features, but I'll be honest with you, they don't
seem as advanced when you have a powerful tool as this one. I highly recommend looking
through what is possible on LazyGit's [Features](https://github.com/jesseduffield/lazygit?tab=readme-ov-file#features)
page.

Anyway, I hope you find this tool to be a much more usable Git client than just
the plain Git CLI. I also hope you'll feel way more powerful using it once you
get comfortable with the lower-level features.
