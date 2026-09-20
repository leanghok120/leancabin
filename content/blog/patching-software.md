---
title: "Patching software"
date: 2026-03-16
---

Recently I switched from Void Linux to Freebsd, I mentioned in my last
blog post that I now use vanilla vim as my primary text editor instead
of GNU Emacs or neovim.

I decided to install vim by building it manually. After installing, I
got an idea to add my own little command to vim. So I did, by editing
the source code which is also known as *patching*.

I added a :freebsd command which just prints out the Freebsd logo with
“i use freebsd btw”.

[![vim-patching.png](https://i.postimg.cc/RZSnr21k/vim-patching.png)](https://postimg.cc/9rkfZ8nb)

It looks pretty cool :o

Patches are not new. In fact, they are actually fairly old. More about
them here: [patches](https://en.wikipedia.org/wiki/Patch_(computing))

Anyway, today I’m going to talk a bit about free software (free as in
free speech).

The fact that we’re even able to modify the behavior of a program to our
own liking is really cool. It’s like taking an iron conjuring spell and
modifying the spell to conjure gold or a sword. It’s really cool and
people should do it more often.

It’s also really fun trying to figure out what a piece of code does
which probably will make you a better programmer. You also get a custom
version of your favorite software by the end of it which is super cool.

So go ahead and clone your favorite program’s repo and modify it to your
liking. It could be something silly like adding a “gud” command to git
so “git gud” becomes a valid command or maybe adding your own keyword to
the C language by editing a tiny C compiler.

The possibility is endless and that’s the beauty of it. You can do
anything you want.
