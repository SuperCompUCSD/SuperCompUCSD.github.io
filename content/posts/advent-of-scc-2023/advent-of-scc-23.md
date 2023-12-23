---
title: "Advent of Supercomputing: Day 23"
date: 2023-12-22
publishDate: 2023-12-14T08:00:00Z
author: ["yuchen"]
draft: false
---

# "Hmm, this music is kinda meh, I wonder what comes next?"

Surrounded by the crowd under the stage of the RIMAC lawn, I reached inside my pocket and unlocked
my phone.  I knew by gut feeling that this was probably now going to work, but I tried it anyways.
Firefox, Google, ...

...

<img src="/post-media/advent-2023-media/day-23/timed-out.png" alt="The connection has timed out" style="height: max(1000px, 100vh);"/>

_Welp, I knew it._  Aside from not having WiFi in the Arena, the data speed also sucks.  _Hmm, I
wonder how the packet loss situation is looking?_  I open Termux, my Linux command line on Android,
and ping the Sun God website:

<img src="/post-media/advent-2023-media/day-23/packet-loss.png" alt="Packet loss is really bad" style="height: max(1000px, 100vh);"/>

Wow, 87% with a max ping time of 22 seconds?  That's _atrocious_.  There's even a duplicate!  I had
never seen that before.  But terrible internet can't stop me!  _It's Mosh time!!!_

# A little history

My time with Mosh goes way back.

I have a backup server across the globe in China that I
self-hosted with a RPi 3B+ that I bought the final year of high school (my rationale for that is
that even under the impossible situation of the entire United States being suddenly erased, my
entire data collection would still be intact.  _Don't ask me why I care about my data when I myself
is in even deeper trouble!  We don't worry about minor non-technical details here._).  I let my
laptop do the backups automatically during the night and all is well.  Except I am my own sysadmin,
so I needed to do some maintenance work every so often once I get an email about another
vulnerability being disclosed in OpenSSH and that I have to update my server _now_ to avoid it being
compromised.

I usually don't mind putting in a couple quick commands, except that my little RPi happy runs
halfway across the globe, and that comes with the problem of having an atrocious round-trip time
(RTT) of 150 ms.  This means that I can't even see my typos until 150 ms in the future, after I
frantically delete and retype, only to find out that I missed a single character in the middle and
have to arrow over to fix it (and then proceed to arrow over one more character than I need.
Urgh!!)

That's not all.  An even worse problem is that somewhere along the entire route to China and back
something often gets wonky and all my packets get lost for minutes at a time.  This means that I
could never tell if my connection just dropped, or that I just needed to wait a bit.  It was
_infuriating_.  So when I discovered Mosh, I knew that it's the thing that I have always wanted.

# Mosh

Mosh, or **Mo**bile **sh**ell, is, according it its [website](https://mosh.org/):

> Remote terminal application that allows roaming, supports intermittent connectivity, and provides
> intelligent local echo and line editing of user keystrokes. \
> Mosh is a replacement for interactive SSH terminals. It's more robust and responsive, especially
> over Wi-Fi, cellular, and long-distance links. \
> Mosh is free software, available for GNU/Linux, BSD, macOS, Solaris, Android, Chrome, and iOS.

In simple terms, it synchronizes the state of the terminal instead of taking the SSH (which works
over TCP) approach of just piping the two character streams over the network and allowing your local
terminal to figure out what it should look like.  This allows it to use UDP instead of TCP to work
under terrible networking environments and to send as little data as possible, since it doesn't need
to send all the terminal history when it is no longer visible.
gg
This allows it to do a lot more cool tricks, too!  Since UDP doesn't work based on connections, your
SSH session doesn't drop even when you roam between different connections.  This became really
useful to me as I often connect to my servers when going around campus, connecting and disconnecting
to the various wireless networks all the time.  I could even put my laptop to sleep on one corner
of campus, get to the other side, wake it up, and continue my SSH session like it has always been
connected!

It also doesn't have a large backing buffer that the TCP protocol needs to retransmit upon a packet
loss, and so your `Control`+`C` works instantaneously when you inadvertently `cat /dev/urandom`.
What's more, it can automatically predict my keystrokes as I type out my command, so I can see
them instantaneously, as if there is zero latency between me and the server!  It will underline
the predicted portion and will retract them if the server reports an inconsistency, but it's seldom
wrong.

...So...

```
$ mosh scc-evans
y5jing@evans:~$
```

_Aha!_

But how exactly does being able to SSH under rough connections help me at the Sun God Festival?

# w3m

_Who needs GUI to use the interwebs?  Noobs._

`$ apt show w3m`

<img src="/post-media/advent-2023-media/day-23/w3m.png" alt="APT description for w3m, the text-based browser" style="height: max(1000px, 100vh);"/>

...And...

<img src="/post-media/advent-2023-media/day-23/sun-god.png" alt="The Sun God website under w3m" style="height: max(1000px, 100vh);"/>

<img src="/post-media/advent-2023-media/day-23/timing.png" alt="The Sun God timing under w3m" style="height: max(1000px, 100vh);"/>

# But is it just a fluke?

I tried to open the Sun God website with my phone's browser instead.

<img src="/post-media/advent-2023-media/day-23/lineup.png" alt="The Sun God lineup page is missing" style="height: max(1000px, 100vh);"/>

<img src="/post-media/advent-2023-media/day-23/lineup.png" alt="The Sun God lineup page is working with w3m" style="height: max(1000px, 100vh);"/>

...Yeah, I don't think so.

_I love Mosh!_

(Of course, Mosh can't handle completely broken connections either:)

<img src="/post-media/advent-2023-media/day-23/lost-contact.png" alt="Mosh lost contact" style="height: max(1000px, 100vh);"/>

But at least it reconnects as soon as it's back up.

# But how does it work?

Check out the "How Mosh works" section on its [website](https://mosh.org/)!  I can't hope to explain
all of it here.  It has a really good explanation, and if you want the juicy details there's a paper
and everything.
