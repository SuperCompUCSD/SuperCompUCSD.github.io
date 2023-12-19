---
title: "Advent of Supercomputing: Day 21"
date: 2023-12-21
publishDate: 2023-12-21:00:00Z
authors: ["paco"]
draft: false
---

There are a lot of commands to spice your terminal aesthetic. They don't serve much of a practical purpose, but they make for dumb fun when setting up servers or messing around with your teammates. We take the 21st day to show our guilt pleasures.

# A mention of classic ones:

These are commands that you may encounter a few months into using the terminal. They are at least the first of flashy commands. 

`cmatrix`, `pipes.sh`, `neofetch`, `htop`, `hollywood`

Purely aesthetic ones are `cmatrix`, `pipes.sh`, and `hollywood`. `cmatrix` is purely aesthetic. It is for that hacker look for characters falling down the screen. Does it serve any purpose besides this? No. `pipes.sh` is a script that runs different pipes through your terminal. It looks nice and is a nice change. It is again, purely aesthetic. The worst (best?) of these is `hollywood`, which will completely cover your terminal in panes opening and closing with nonsensical patterns and random uses of commands. Any of these commands are usually screensavers for me, but this one is too much...

Semiuseful ones are `neofetch`, which will give some small summary of the system being used, and `htop` which is mostly used to monitor processes and serves high practical value. Like `top`, it monitors processes and some resources, but it is very customizable and has better formatting. It is a must when using servers.


### Useful ones

`btop`

So the `top` command really inspired a whole branch of commands that monitor things like I/O, download/upload speed, processes, disk usage, etc. I swear you can find a different one written in every language. With an additional five minutes I came accross `zenith`, `ytop`, `bottom`, `gotop`, and the list goes on and on.

A very good on is `btop`. It's very customizable, which is another reason to like `htop` as well. Though I believe `btop` has subjectively better presentation if you want to see multiple things at once, and it includes download/upload while `htop` doesn't. This is really subjective at the end of the day.

btop |  Options
:-------------------------:|:-------------------------:
![btop_image](/post-media/advent_2023_21/btop.png)  |  ![btop_options](/post-media/advent_2023_21/btop_options.png)





`figlet`

To those that already know this command you might think "*figlet*?! A *useful* command? Are you kidding me?". I'm serious, it makes for better documentation. Trying to scroll through code/scripts gets difficult when scrolling for a long time. Or sometimes you want to make a name/section **pop**!. And `figlet` helps with that. It takes in normal sized text and gives an output larger than that.

```
$ figlet "Home Server"
 _   _                        ____                           
| | | | ___  _ __ ___   ___  / ___|  ___ _ ____   _____ _ __ 
| |_| |/ _ \| '_ ` _ \ / _ \ \___ \ / _ \ '__\ \ / / _ \ '__|
|  _  | (_) | | | | | |  __/  ___) |  __/ |   \ V /  __/ |   
|_| |_|\___/|_| |_| |_|\___| |____/ \___|_|    \_/ \___|_|   
                                                             
```
Figlet allows one to put different fonts, called figlet fonts and you can see which are installed the command `showfigfonts`, showing you a preview too. Now there's supposed to be replacement to figlet called `toilet`. It offers pretty much similar outputs but with colors. Both really haven't seen much development for a long time though.




# Purely Aesthetic

`timg`, `asciiquarium`, `cbonsai`, 

`timg` outputs video or image on the terminal through colored blocks. It's pretty useless but it's fun. `asciiquarium` will give a small shallow ocean with ascii fish, shark, and a bit more. And `cbonsai` will grow/display a small bonsai tree. It brings peace to the raging and frustrated mind of a programmer/sysadmin. 

timg |  asciiquarium | cbonsai
:-------------------------:|:-------------------------:|:----------------------|
![btop_image](/post-media/advent_2023_21/btop.png)  |  ![btop_options](/post-media/advent_2023_21/btop_options.png) | 




Honestly this is only the shallow waters terminal ricing. Which in itself is a smaller subset of ricing. Ricing being the term for making your computer look nice. While poking around I was reminded of a good list of resources here: [awesome-ricing](https://github.com/fosslife/awesome-ricing).

(Please stop putting picture of attractive video game characters on my *.bashrc* admins. I open the terminal around my family.)
