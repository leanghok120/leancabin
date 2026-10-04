---
title: "A reflection on software"
date: 2026-10-04
---

Our hardware has gotten much better over the years. In contrast, our software
has gotten much worse. This makes the technological advances in the hardware
world almost meaningless (by hardware, I'm referring to the cpu, gpu, memory in
our computer).

Modern day cpu can process at 10x speed compared to the ones in 2011 and yet our
experience with modern day software seems to have stayed the same. In fact,
it has gotten worse.

Opening a simple text editor like vscode takes almost a second. To put in comparison,
Casey Muratori (the person behind Handmade Hero) has a [recording](https://www.youtube.com/watch?v=TlMt055gThA)
showcasing the speed of Visual Studio 6 on a 20 year old machine. As shown in the video, the
machine had less than a gigabyte of memory with only 1 cpu core. Besides this, the video shows
how quick (instant) updating the watch window was in Visual Studio 6. Fast forward 20 years later,
this type of speed and efficiency is lost.

Another problem is the memory usage of the applications. Whoever thought
it was a good idea to bundle an entire Chromium instance to disguise a web app
as a desktop app probably has something to do with this.

Electron apps (Discord, Slack, Vscode, Spotify, Obsidian, and a thousand more) are
infamous for hogging up memory because they bundle an entire Chromium instance with the application. These
apps usually require atleast 150MB of ram to open which might not seem a lot in the modern day but if
we were to compare it to software back in the days, 150MB on idle seems stupidly unnecessary and inefficient.

So what went wrong? How did we go from fast and efficient software to this abomination?

I believe the answer lies in our carelessness. We stopped caring about how much system resources
our software is using. As a result, we end up writing bloated software thinking it's normal. People
then write software on top of our already bloated software creating even more bloated software.

Alright then, what's the solution?

Use better software. Switch to linux, use a windows manager, find lightweight clients for software you use.
If there's none, then write your own tiny client.
