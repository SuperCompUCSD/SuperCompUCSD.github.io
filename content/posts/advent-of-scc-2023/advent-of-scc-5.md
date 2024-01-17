---
title: "Advent of Supercomputing: Day 5"
date: 2023-12-05
publishDate: 2023-12-05T08:00:00Z
author: ["matei"]
draft: false
---

# What `thefrick`?

Have you ever had one of those moments when you've coding on your computer way too late?

I know I have!

Nothing quite scratches my itch of coding like staying up at ridiculous hours of the night digging myself out of the 27th bug
I've found whilst working on the side-side-project that popped up as I was busy procrastinating on my homework. However, every
pursuit of such highs has its costs.

For starters, staying up late can be bad for your health. But ignoring that aspect, staying up late can make you lose focus and make
silly mistakes that you otherwise wouldn't do. Not to mention you might get a little bit more pissed.

So what if your computer shared the same mood with you whilst in this extremely relatable and common situation? Now you can!

## Meet `thefuck`

Let's say I'm up real late and fixing up my dotfiles for my home directory. I naturally start at the home directory and look through it
to find my config directory for Emacs:

```sh
storm_firefox1 on Storm Prism   on ☁️   
>>> ls
Applications			Downloads			Mail				Org				Repos
Applications (Parallels)	Endgame.mp4			Movies				Parallels			Virtual Machines.localized
Databases			GPG				Music				Pictures			
Desktop				Games				Obsidian			Projects
Documents			Library				OneDrive			Public
```

Ah, crap. I meant the hidden files! Hold up, let me tell the computer that I'm pissed this didn't work so well.

```sh
storm_firefox1 on Storm Prism   on ☁️   
>>> alias frick="fuck"
>>> frick
ls -lah [enter/↑/↓/ctrl+c]
```

My, you're so right! I totally meant to use the `-a` argument to show hidden files, too!

Alright, finally found my Emacs directory. I'll make a new branch in my dotfile Git repo for Emacs to setup my Org-mode files correctly
(Don't worry about what that is, that rabbit hole is for another blog post). Alright...

```sh
storm_firefox1 on Storm Prism ./.emacs.d on 🌱 master on ☁️   
>>> git checkout setup-org
error: pathspec 'setup-org' did not match any file(s) known to git
```

Frick!

```sh
storm_firefox1 on Storm Prism ./.emacs.d on 🌱 master on ☁️   
✗>>> frick
git checkout -b setup-org [enter/↑/↓/ctrl+c]
Switched to a new branch 'setup-org'
```

Oh, you're right, I didn't make the branch yet. Thanks, PC!

So as you can probably tell, this really nifty command lets you just instantly fix common typos whenever you're on the shell. It's got a ton
of community-made rules enabled by default that cover a lot of common screw-ups, including the two above (which are my most common). It's also
really fast, too, so you don't need to worry about waiting a long time for ChatGPT to tell you what the right command is for 80% of use cases.

It's got a lot of rules and common fixes, including but not limited to:
- Adding `chmod +x` to a shell script you just made if you forgot to
- Fixing typos for a myriad of package managers, for distributions or even
  programming languages. It covers `yum`, `apt`, `pacman`, `composer`, `yarn`, you name it!
- Asking you to login to Docker Hub if you forgot to do it beforehand
- Adding `--std=c++11` to `g++` in case your C code ended up not compiling,
  or if it's marked as needing C++11 to run.
- Running a previous command from your shell history if you made a tiny typo
- Adding `sudo` to a command if you forgot to run it as root
- Adding `-p` to `mkdir` if you happened to forget to make the parent
  directories

There are also community-made rules you can use that are not in the core program. And hey, you can make your own with a simple Python module guide.
And if any of the rules annoy you, you can disable them via config.

If you wanna try out this command, most operating systems have it in their
repositories by default. For instance, on macOS:

```sh
brew install thefuck
```

Or Arch Linux:
```sh
sudo pacman -S thefuck
```

If you wanna see even more details on this cute little tool,
[check out the GitHub repo](https://github.com/nvbn/thefuck).

That being said, I know that this tool might not work sometimes, and sometimes
you can definitely forget what specific command to run. In those cases, you can
ping ChatGPT from the shell to help you write out shell commands. There used to be
a technical preview a while back from GitHub, but now you'll probably need to DIY it.
You can use [this project](https://github.com/liCells/chatgpt-cli) as a good starting point
to replace that functionality. This works quite well for more complex requests:

```sh
storm_firefox1 on Storm Prism   on ☁️   
>>> ??
? Enter your question » Tar archive "Downloads" directory but only files within it, not the directory itself
tar -cvf Downloads.tar Downloads/*
? Do you want to execute this command? » Y/n/s(suggestion)/e(explain)/c(Copy to Clipboard)
```

You can setup the `??` alias like so:
```sh
alias \?\?='chatgpt-cli -c $SHELL -t $OPENAI_KEY -C'
```

Hopefully, with the help of the above commands, you'll have less trouble navigating the shell and getting your work done at the
best interface for any Linux fanatic: the shell.

I should probably go to sleep instead of doing all this, but it's more fun this way. Plus, this lets me be lazy any time of the day! 😉