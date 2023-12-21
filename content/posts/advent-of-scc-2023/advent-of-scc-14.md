---
title: "Advent of Supercomputing: Day 14"
date: 2023-12-14
publishDate: 2023-12-14T08:00:00Z
author: ["yuchen"]
draft: false
---

# A painful debugging journey

So you have finally written the best solution for your CS homework.  Or so you think -- you haven't
actually tested it yet.  That's the easy part.  Right?

_Wrong!_

Turns out that your program somehow doesn't work deterministically.  You get a segfault, run it
again, and it magically disappears!  But after many reruns, it came back!  You go to your trusty
friend GDB to run the program and hopefully catch it misbehaving.  After a few attempts, you were
successful.  You set a breakpoint and continue, looking at the value that you suspect is getting
corrupted.  It's going great, but, _click_ -- you mistype, and choose the wrong command from
history, continuing past the break point and killing the program.  _Nooo!_  Well, time to start from
the beginning

_One hour later_

"Uhh!" You sit at your desk, hands cramping from running your program over and over again, shouting
from the agony of the painstakingly slow progress as you make a tiny bit of progress each time you
were able to reproduce the bug.  "But this is going to take ages!"  You go on Gradescope and check
your assignment deadline.  "Only an hour left??"  The clock is ticking and sweat drips from your
face.  If only there's a better way!  A way to keep debugging that one run, to go back and forth
through time, as if you are time travelling --

## Enter [`rr`](https://rr-project.org/)

> rr aspires to be your primary C/C++ debugging tool for Linux, replacing — well, enhancing — gdb.
> You record a failure once, then debug the recording, deterministically, as many times as you want.
> The same execution is replayed every time.
>
> rr also provides efficient reverse execution under gdb. Set breakpoints and data watchpoints and
> quickly reverse-execute to where they were hit.

"Oh, okay, let me install `rr` real quick..."

```
$ sudo apt install -y rr
$ sudo sysctl kernel.perf_event_paranoid # check default level so you may reset it later
$ sudo sysctl kernel.perf_event_paranoid=1
```

## Time to use your newfound power to debug your homework!
<script src="https://asciinema.org/a/rPeAhBiloZdjskrcmADZmDLRl.js" id="asciicast-rPeAhBiloZdjskrcmADZmDLRl" async></script>
