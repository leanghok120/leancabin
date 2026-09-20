---
title: xlib is a bad library
date: 2025-09-24
---

Xlib is a C library for communicating with the X server and it’s a bad
library. I’ve used Xlib in a couple of my projects like my buggy tiling
windows manager and the simple x11 bar I wrote in my previous blog post.
The main problem with Xlib is that it’s unintuitive, there’s stuff that
you *just* need to know like having to do an `XFlush(dpy)` after drawing
something or an `XMapWindow()` to display a window after creating one.
It’s not too bad once you get used to the library but for beginners,
it’s really hard to pick up. Another problem is that there’s not much
good documentation for Xlib that tells you the stuff you *need* to know.
Most of the Xlib documentation is just a more crappy version of the man
page and the man page doesn’t actually tell you what you need to know,
it just tells you the function arguments and returns. Lastly, Xlib is a
very low level library so there’s a lot of boilerplate code needed to do
very simple stuff. I’m talking like 30 lines of code to just display a
window on the screen (including handling signals and wm quit event).

Lately, I’ve been trying to find an Xlib + Xft wrapper that could reduce
the amount of boilerplate I need to write and the best one I’ve found is
[suckless’s libsl](https://git.suckless.org/libsl/log.html) which is a
really small wrapper on top of Xlib and Xft which they use in quite a
couple of their projects.
