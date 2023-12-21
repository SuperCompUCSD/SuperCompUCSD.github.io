---
title: "Advent of Supercomputing: Day 21"
date: 2023-12-21
publishDate: 2023-12-21:00:00Z
authors: ["paco"]
draft: false
---



# Sweet and Sugary Commands

There are a lot of commands to spice your terminal aesthetic. Some of them don't serve much of a practical purpose, but all of them are fun when setting up servers. Here are some commands for when you are bored.

### A mention of classic ones:

These are commands that you may encounter a few months into using the terminal. They are at least the first of flashy commands. 

**`cmatrix`**, **`pipes.sh`**, **`neofetch`**, **`htop`**, **`hollywood`**

Purely aesthetic ones are `cmatrix`, `pipes.sh`, and `hollywood`. `cmatrix` is purely aesthetic. It is for that hacker look for characters falling down the screen. Does it serve any purpose besides this? No. `pipes.sh` is a script that runs different pipes through your terminal. It looks nice and is a nice change. It is again, purely aesthetic. The worst (best?) of these is `hollywood`, which will completely cover your terminal in panes opening and closing with nonsensical patterns and random uses of commands. Any of these commands are usually screensavers for me, but this one is too much...

Semiuseful ones are `neofetch`, which will give some small summary of the system being used, and `htop` which is mostly used to monitor processes and serves high practical value. Like `top`, it monitors processes and some resources, but it is very customizable and has better formatting. It is a must when using servers. Here is what they look like:

<img src="/post-media/advent-2023-media/day-21/classic.gif" alt="class-commands" style="height:600px;"/> 

Few of these commands will be installable via package manager. Maybe it is because package maintainers don't see much practical purpose for some of these, and who can blame them? So if you're on mac try the typical `brew search <package-name>`, or if you're using another unix/unix-based machine use the appropriate package manager. Or you can find their source as well, like github repos for [pipes.sh](https://github.com/pipeseroni/pipes.sh), [hollywood](https://github.com/dustinkirkland/hollywood), [cbonsai](https://gitlab.com/jallbrit/cbonsai), and probably all of the ones mentioned in the rest of the post actually.

### Useful ones

**`btop`**

So the `top` command really inspired a whole branch of commands that monitor things like I/O, download/upload speed, processes, disk usage, etc. I swear you can find a different one written in every language. With an additional five minutes I came accross `zenith`, `ytop`, `bottom`, `gotop`, and the list goes on and on.

A very good on is `btop`. It's very customizable, which is another reason to like `htop` as well. Though I believe `btop` has subjectively better presentation if you want to see multiple things at once, and it includes download/upload while `htop` doesn't. This is really subjective at the end of the day.

btop |  Options
:-------------------------:|:-------------------------:
<img src="/post-media/advent-2023-media/day-21/btop.png" alt="btop.png" style="height:350px;"/> | <img src="/post-media/advent-2023-media/day-21/btop-options.png" alt="btop-options.png" style="height:350px;"/> 




**`figlet`**

To those that already know this command you might think "*figlet*?! A *useful* command? Are you kidding me?". I'm serious, it makes for better documentation. Trying to scroll through code/scripts gets difficult when scrolling for a long time. Or sometimes you want to make a name/section **pop**! And `figlet` helps with that. It takes in normal sized text and gives an output larger than that.

```
$ figlet "Home Server"
 _   _                        ____                           
| | | | ___  _ __ ___   ___  / ___|  ___ _ ____   _____ _ __ 
| |_| |/ _ \| '_ ` _ \ / _ \ \___ \ / _ \ '__\ \ / / _ \ '__|
|  _  | (_) | | | | | |  __/  ___) |  __/ |   \ V /  __/ |   
|_| |_|\___/|_| |_| |_|\___| |____/ \___|_|    \_/ \___|_|   
                                                             
```
Figlet allows one to put different fonts, called figlet fonts. With the additional command `showfigfonts` you can view a preview of what all the fonts installed look like. I'm willing to bet [Expanse](https://www.sdsc.edu/services/hpc/expanse/expanse_architecture.html) used it for their login as well.

```text
[pacolord laptop ~]$ figlet -cf slant "EXPANSE"
                    _______  __ ____  ___    _   _______ ______
                   / ____/ |/ // __ \/   |  / | / / ___// ____/
                  / __/  |   // /_/ / /| | /  |/ /\__ \/ __/   
                 / /___ /   |/ ____/ ___ |/ /|  /___/ / /___   
                /_____//_/|_/_/   /_/  |_/_/ |_//____/_____/   
                                                               
[pacolord laptop ~]$ ssh expanse
Welcome to Bright release         9.0

                                                         Based on Rocky Linux 8
                                                                    ID: #000002

--------------------------------------------------------------------------------

                                 WELCOME TO
                  _______  __ ____  ___    _   _______ ______
                 / ____/ |/ // __ \/   |  / | / / ___// ____/
                / __/  |   // /_/ / /| | /  |/ /\__ \/ __/
               / /___ /   |/ ____/ ___ |/ /|  /___/ / /___
              /_____//_/|_/_/   /_/  |_/_/ |_//____/_____/

--------------------------------------------------------------------------------

Use the following commands to adjust your environment:

'module avail'            - show available modules
'module add <module>'     - adds a module to your environment for this session
'module initadd <module>' - configure module to be loaded at every login

-------------------------------------------------------------------------------
[myuser@login02 ~]$ 
```

Huh... would you look at that. You can find more figlet fonts [here](https://github.com/xero/figlet-fonts). Then just move then to `/usr/share/figlet` (or wherever they are stored). Now there's supposed to be replacement to figlet called `toilet` made by the same guys who made the cacalib, a library for outputting images/video through ascii. It offers pretty much similar outputs but with colors. Both really haven't seen much development for a long time though.


# Purely Aesthetic

`timg`, `asciiquarium`, `cbonsai`, 

**`timg`** outputs video or image on the terminal through colored blocks. It's pretty useless but it's fun. 

image |  timg
:-------------------------:|:-------------------------:|:----------------------|
 <img src="/scc-2023-team-logo.png" alt="scc-2023-team-logo" style="height:400px;"/> | <img src="/post-media/advent-2023-media/day-21/timg-scc-2023-team-logo.png" alt="timg-scc-2023-team-logo" style="height:400px;"/> 


**`asciiquarium`** will give a small shallow ocean with ascii fish, shark, and a bit more. And **`cbonsai`** will grow/display a small bonsai tree. It brings peace to the raging and frustrated mind of a programmer/sysadmin. 

 asciiquarium | cbonsai
:-------------------------:|:-------------------------:|
 <img src="/post-media/advent-2023-media/day-21/asciiquarium.gif" alt="asciiquarium" style="height:350px;"/> | <img src="/post-media/advent-2023-media/day-21/cbonsai.gif" alt="cbonsai" style="height:350px;"/> 



There exists prompts, projects like `cowsay`, `fortune`, `lolcat` and so many more. Honestly this is only the shallow waters terminal ricing. Which in itself is a smaller subset of ricing. Ricing being the term for making your computer look nice. While poking around I was reminded of a good list of resources here: [awesome-ricing](https://github.com/fosslife/awesome-ricing). Have fun with this&mdash; that's what it is here for.

(*P.S.* To the guy putting images of attractive video game characters and calling on them via my *.bashrc* with *timg* please stop. I open the terminal around my family.)
