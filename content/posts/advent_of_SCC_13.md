---
title: "Advent of the SuperComputing Club: Day 13"
date: 2023-12-13
author: ["paco"]
publishDate: 2023-13-25T08:00:00Z
draft: true 
---


# Day 13: xev/wev

Sometimes when setting up a keyboard shortcut or in some application finding the correct *keycodes* is important. For that reason `xev` can **print X events*. Equivalently for wayland there is `wev` to **show wayland events**.


```bash
wev
```

This will open a checkerboarded window where you can interact and test input. A previous usecase of mine has been when setting up keybdinginds for `dwl`, a wlr-roots based wayland compositor that forks off `dwm`, a [suckless](https://suckless.org/) window manager.

It also reads the placement of the mouse on the screen whenever it is moved, making it for a useful way to read location of the monitors, though `xrandr` or `wlr-randr` is probably more suitable for that.

In order to add a keybinding one must edit the source code. A lot of the keys are reffered to as `XKB_KEY_<KEY-HERE>`. Some of these are straightforward, like `XKB_KEY_a` or `XKB_KEY_A` for lowercase *a* and upper case *A*. But some were trickier like the *\^* symbol:

```bash
wev
...
[14:     wl_keyboard] key: serial: 44043; time: 18982926; key: 15; state: 0 (released)
                      sym: asciicircum  (94), utf8: ''
```

From this I can see the key is *asciicircum*, and I edited my own config to have `XKB_asciicircum` do whatever I wished it to do.
