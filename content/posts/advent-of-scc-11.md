---
title: "Advent of Supercomputing: Day 11"
date: 2023-12-11
publishDate: 2023-12-11T08:00:00Z
author: ["paco"]
draft: false
---

# File sharing with <span>0x0.st</span> and magic-wormhole

### 0x0.st

The *Null Pointer* is a good site for file sharing. So sharing images, videos, textbooks, gifs, etcetera can be very simple with [`0x0.st`](https://0x0.st). The site offers flexibility of having a place to quickly upload and share SFW files. The best part? You can do it all from your terminal with `curl`.

```bash

# upload
$ curl -F'file=@SCC_23_team_logo.png' https://0x0.st
https://0x0.st/H3wm.png

# download
$ wget https://0x0.st/H3wm.png 


# Upload, setting an expiration an hour from now:
$ curl -F'file=@funny_team_logo.png' -Fexpires=1 https://0x0.st
https://0x0.st/H3wa.png

```


[`0x0.st`](https://0x0.st) does not allow past file sizes of 512MiB as well as some executables and some files described on the site. As some good advice if you don't feel comfortable sharing the photos with the whole world you can encrypt them in some way, such as with `gpg`. But I personally like the way [bugswriter](https://www.bugswriter.com/) does it: `zip` up a few files and `zipcloak` them; zipping files together has the benefit of allowing one to have one url for one file, and by zipping it is at least compressing the file size a little bit. `zipcloak` encrypts them, so they're not easily viewable:

```bash
# Example: taking gifs, zipping them, and using zipcloak
$ zip -r screensaver_gifs.zip *.zip
$ zipcloak screensaver_gifs.zip 
Enter password: 
Verify password: 
encrypting: 100%Animosity.gif
encrypting: ErenReencounterWithColossal.gif  
encrypting: FlouriteEye.gif
encrypting: ScaryMob.gif
encrypting: cat_breakdancing.gif
encrypting: goodBoyMob.gif
encrypting: princessMononoke.gif

$ curl -F"file=@screensaver_gifs.zip" 0x0.st # this will use http and not https btw

# say this is someone else or from another computer that knows the password:

$ wget 0x0.st/H3wQ.zip
$ unzip H3wQ.zip 
Archive:  H3wQ.zip
[H3wQ.zip] 100%Animosity.gif password: 
  inflating: 100%Animosity.gif       
  inflating: ErenReencounterWithColossal.gif  
  inflating: FlouriteEye.gif         
  inflating: ScaryMob.gif            
  inflating: cat_breakdancing.gif    
  inflating: goodBoyMob.gif          
  inflating: princessMononoke.gif    
# Now we can view our files
```


[0x0.st](https://0x0.st) also used to offer URL-Shortening, but it has since been disabled. Check with one of its mirrors though.


Another alternative to this file sharing site is [*`transfer.sh`*](https://transfer.sh), which offers most of the same features as 0x0.st. It offers a max upload of 10GB for the cost of only storing a default of 14 days. There are some more options that it might be worth checking out over `0x0.st`, such as setting max downloads or making it easier for normal people to upload with their examples.

The site [tmpfiles.org](https://tmpfiles.org) has a maximum file size of 100Mb and only an hour, but allows you to upload from their site gui or with `curl`, and [imgdb.net](imgdb.net) boasts to be a "*resilient Image Archive*". One this site one can view all images posted, but as a downside there's no direct way to upload with curl. I would still recommend [0x0.st](https://0x0.st) and [transfer.sh](https://transfer.sh) over both of these. These two also have their code so you can mirror them as well, which you should totally do:

[Null Pointer (Mia's gitea)](https://git.0x0.st/mia/0x0) and [transfer.sh github](https://github.com/dutchcoders/transfer.sh/).
In any of these cases making a script will make your life easier.

### magic-wormhole

Now all that was previously good. But sometimes you want send a specific file to someone physically nearby. What do you do? Now you can take a pen drive and store that file, then give them the drive. You could use `0x0.st` but you worry about the file size or perhaps the contents. Maybe use netcat [(video example)](https://www.youtube.com/shorts/1j17UBGqSog), but that's just too confusing. You can run `python -m http.server 8000` to set up an ftp server, then figure out your ip with `ip a` or `nmcli`, but that's 2 steps and anyone can just view it (this is a good option acutally). You can set up `samba`, `nfs`, or some file sharing service. There's a lot of options, but... is this seriously necessary? I just want to send one or two files, and probably never agiain. It should be so simple... so why does it not feel this way?

Sending and receiving is as simple as two commands:
```bash
$ wormhole send file.mp4

$ wormhole receive
```

Now the wormhole package, called `magic-wormhole` **not** wormhole, and can be installed on most systems via the package manager: `xbps, brew, pacman, ...`. The sender and receiver must both have this package. Here's an example:

```bash
[SCC Documentation]$ wormhole send mpas_atmosphere_users_guide_8.0.1.pdf 
Sending 9.1 MB file named 'mpas_atmosphere_users_guide_8.0.1.pdf'
Wormhole code is: 4-paramount-backward
On the other computer, please run:

wormhole receive 4-paramount-backward
```
And from another system in the same network:
```bash
[Company_System]$ wormhole receive 4-paramount-backward
Receiving file (9.1 MB) into: 'mpas_atmosphere_users_guide_8.0.1.pdf'
ok? (Y/n): y
Receiving (->tcp:192.168.0.233:44059)..
100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 9.13M/9.13M [00:00<00:00, 46.3MB/s]
Received file written to mpas_atmosphere_users_guide_8.0.1.pdf
```

It's pretty straightforward, as it should be.
`croc` is another command that is essentially the same as `magic-wormhole`. It does have some more options, and most of the `magic-wormhole` commands are identical on `croc`, so it is a good substitution. 
