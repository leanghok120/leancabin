---
title: "Tutorial: write your own bar"
date: 2025-09-06
---

In this blog post, we’re going to be writing a simple X11 bar in C with
xlib. By the end of this blog post you will understand the basics of
xlib, ewmh, and have a cool minimal bar to play with.

[![myownbar.png](https://i.postimg.cc/qMRs6srg/myownbar.png)](https://postimg.cc/Cd3BWfzV)

Note: right now the bar only displays the current date and time.

Full code: <https://github.com/leanghok120/simplebar>

## Assumptions

This blog post assumes you know the basics of C and the X windowing
system.

## Creating a simple window

We’ll start by creating our bar window:

```c
    #include <X11/Xlib.h>
    #include <X11/Xatom.h>
    #include <stdio.h>
    #include <unistd.h>

    int main() {
      // init
      Display *dpy = XOpenDisplay(NULL);
      if (dpy == NULL) {
        fprintf(stderr, "cant open display lol bozo");
        return 1;
      }
      int scr = DefaultScreen(dpy);
      int width = DisplayWidth(dpy, scr);
      int height = 20;

      // create the bar window
      Window bar = XCreateSimpleWindow(dpy, RootWindow(dpy, scr), 0, 0,
                                       width, height, 0,
                                       BlackPixel(dpy, scr), WhitePixel(dpy, scr));
      XStoreName(dpy, bar, "cool bar");

      // show the bar window
      XMapWindow(dpy, bar);
      XFlush(dpy);

      sleep(5);

      // clean up
      XDestroyWindow(dpy, bar);
      XCloseDisplay(dpy);

      return 0;
    }
```

The program above should create a window with a width of your screen and
a height of 20 but upon running the program you should see that the bar
window is positioned like a normal window.

If you’re using a tiling windows manager, you should see the bar window
tiled like this:

[![tempbar2.png](https://i.postimg.cc/SxfPd0RD/tempbar2.png)](https://postimg.cc/MnXtZFyQ)

This happens because we haven’t told the windows manager that our window
is supposed to be a bar so our windows manager just treats it like a
normal window.

## Setting ewmh atoms

In X11, EWMH is a standard that windows managers and other x11 apps use
to communicate with each other. For example: by setting a value for
these EWMH atoms, the windows manager will be able to know if your
program is a normal window or a bar.

We can tell the windows manager that our window is a bar by setting a
specific EWMH atom called `_NET_WM_WINDOW_TYPE` to
`_NET_WM_WINDOW_TYPE_DOCK` like so:

```c
      // create the bar window
      // ...

      // set bar windows type to dock
      Atom wm_window_type = XInternAtom(dpy, "_NET_WM_WINDOW_TYPE", False);
      Atom wm_window_type_dock = XInternAtom(dpy, "_NET_WM_WINDOW_TYPE_DOCK", False);
      XChangeProperty(dpy, bar, wm_window_type, XA_ATOM, 32, PropModeReplace, (unsigned char *)&wm_window_type_dock, 1);

      // show the bar window
      // ...
```

By doing this, your bar window becomes a dock and the windows manager
will be able to position it like a dock (in most tiling windows manager,
they just skip positioning dock windows)

When you compile and run the program, there should be a white window at
the top of your screen.

## Adding texts

Now that we have a blank bar, it’s time to add in the information. we’ll
be adding in the current date and time.

Believe it or not, this part is probably the most complicated part of
the tutorial because of how drawing texts works in xlib.

We’ll start by loading the font in:

```c
      // set bar windows type to dock
      // ...

      // add text
      Font font = XLoadFont(dpy, "fixed");
      GC gc = XCreateGC(dpy, bar, 0, NULL);
      XSetFont(dpy, gc, font);
      XSetForeground(dpy, gc, BlackPixel(dpy, scr));

       // ...
      // clean up
      XUnloadFont(dpy, font);
      XFreeGC(dpy, gc);
      XDestroyWindow(dpy, bar);
      XCloseDisplay(dpy);
```

now we will add "hello world" to our bar window by replacing:

```c
    sleep(5)
```

with:

```c
      // show the bar window
      // ...

      // add text to the window
      while (1) {
        XDrawString(dpy, bar, gc, 15, 10, "hello world", 11);
        XFlush(dpy);
        sleep(1);
      }

      // ..
```

running the program, we should see a white window with a black text of
"hello world".

let's replace "hello world" with the current date and time:

```c
      // add text to the window
      char buf[64];
      while (1) {
        time_t now = time(NULL);
        strftime(buf, sizeof(buf), "%a %d %b %Y %I:%M:%S %p", localtime(&now));

        XClearWindow(dpy, bar); // avoid overlapping text
        XDrawString(dpy, bar, gc, 15, 10, buf, strlen(buf));
        XFlush(dpy);

        sleep(1);
      }
```

Now when we run the program, we should see our bar with the current date
and time.

## Making the bar a bit more beautiful

We can all agree that currently our bar looks like shit. We can try to
fix this by centering the date and time.

In order to do this, we need to get the width of the date and time text
and subtract it from our width and divide it by 2:

```c
        // (in our loop)
        int text_width = XTextWidth(XQueryFont(dpy, font), buf, strlen(buf));
        int text_x = (width - text_width) / 2;
        int text_y = height - 6;

        XClearWindow(dpy, bar); // avoid overlapping text
        XDrawString(dpy, bar, gc, text_x, text_y, buf, strlen(buf));
        XFlush(dpy);
```

Running the program, we should see our beautiful bar:

[![finalbar.png](https://i.postimg.cc/KjxzydXq/finalbar.png)](https://postimg.cc/k6Y9QYgK)

We now have a simple and minimal bar.

## Lastly

You might notice that the code in the repo contains a bit more lines
than the code in this blog post, this is because the code in the repo
also handles SIGINT and SIGTERM.

You might also notice that our bar has a slight delay where the current
date and time shows up a second after the window is mapped. This is
because we are just blindly drawing our text after every second. If we
want the text to show up as soon as the window is mapped, we would need
to also draw the text when our window receives an Expose event.

And that’s pretty much it, we just built a minimal X11 bar in C. From
here on out, you can expand it to show system stats, workspaces or make
it look even better.

[![barblack.png](https://i.postimg.cc/vBWKVK1d/barblack.png)](https://postimg.cc/Hck3GB4S)
