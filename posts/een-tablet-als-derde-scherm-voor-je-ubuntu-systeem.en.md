<!--
.. title: How to use a tablet as a third screen for your Ubuntu system
.. slug: a-tablet-as-third-screen-for-ubuntu
.. date: 2018-09-03 21:02:00 UTC+02:00
.. tags: ubuntu, linux, tips
.. category:
.. link:
.. description: How to use a tablet as an external screen.
.. type: text
-->

At work I have a docking station. That way I can use two
external screens with my laptop. And I find this extremely useful.
I put a terminal window here, some source doe there, a similar
source file over there, and I need to write a unit test while
looking up things on Stack Overflow. So those three screens are
easily filled.

Sometimes I work from home. I don't have a docking station at my
place, and I can use only one external screen. This works as well,
but often I miss the extra screen real estate.
But we have a tablet at home, which is mainly used by our children,
who are typically away or asleep when I code. So how difficult could
it be to use this tablet as an external screen?

![a tablet as external screen](/galleries/3rd_Screen/3rdscreen.jpg)

<!-- TEASER_END -->

This was of course somewhat more difficult than I initially thought.
(A detail I might have failed to mention is that I am running
Ubuntu 18.10 on my laptop.) It took some messing around, but
now I have a working system. It is not the world's most beautiful
solution, but it will do for me.

## Thank you, Mike

I found my inspiration in a blog post from
[https://mikescodeoddities.blogspot.com/2015/04/android-tablet-as-second-ubuntu-screen.html](Mike's Coding Oddities).
It needed some tinkering, because I need a 3rd screen, Mr. Mike
needs a 2nd screen.

The proposed solution does not work for Wayland. Which is a shame,
becuase Waylands sounds so much cooler than the outdated Xorg.
But you need to make some sacrifices if you want more screen space.

## My configuration

The on-board screen of my laptop is 'called' eDP-1. The external
screen is HDMI-1. I always put the external screen at the right
side of my laptop, and I want the tablet to the right of the
external screen. Both laptop and external screen have a resolution
of 1920x1080. The tablet has a screen resolution of 1920x1200.
I will have 1920x120 unused tablet-pixels because of the solution
I choose, but it is the best I can do for now.

(Finding the names of the outputs, eDP-1 and HDMI-1, was not easy.
I ended up installing and running [arandr](https://christian.amsuess.com/tools/arandr/) to find
out how the outputs are called, but there are probably better ways
to do this.)

## The script

This is the script I use:

```
#!/bin/bash

sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 3840x1080+1920+0/3840x1080+1920+0
sleep 3
sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 1920x1080+1920+0/3840x1080+1920+0
x11vnc -clip 1920x1080+3840+0 -nocursorshape -nocursorpos
```

You need to use `sudo`. Omitting sudo does not produce error
messages, but it will not work either.

As I understand it, the first line ensures that the screen space
for my 3 screens all together becomes 5740x1080 pixels. The external screen
(HDMI-1) became twice as wide (3840x1080), and is located to the right of the
laptop screen (+1920+0).

If you would only execute the first line, the external screen would 'slide'
as 'view port' over the wider logical screen when you would move the mouse
cursor to the left side or right side of the screen.

The `sleep 3` just waits for three seconds, and the next line limits the
area of the logical screen that can be displayed on the physical screen by
1920x1080 pixels. That way, you can move your mouse cursor outside the
visible part of the screen, avoiding the view port to slide.

At last I start a VNC server, to serve the invisible area of the external
screen. The options `-nocursorshape` and `-nocursorpos` arrange that the
mouse cursor is controlled by the VNC server (laptop), and not by the client
(tablet).

Now you need to install a VNC client on the tablet, to connect to your PC. I use
[VNC viewer](https://play.google.com/store/apps/details?id=com.realvnc.viewer.android),
but other viewers are availabe.

Please note that the VNC connection I created this way, is not secured,
neither encrypted. That's fine for testing, or when you are in complete control
of your network. But if that is not the case, use ssl, and put a password
on your VNC connection. (I don't know yet how to tho this, but I guess if you
look at the man page of x11vnc, you will find some hints if you search for
`-ssl` or `-rfbauth` )

## Room for improvement

This solution works reasonably well, but there are some limitations. First of all
your external screen and your tablet for the Linux environment together are one screen.
That has the annoying consequence that if you maximize a window, that window
scattered across those two screens. You can work around there
instead of maximizing your window 'snap left' and 'snap right' too
use; the shortcuts for that are superkey + arrow left and right
superkey + arrow right.

I think it should be possible to make your system think that
there are really three screens, and that you can then configure all three separately
(resolution, orientation, position). There is one
[dummy video driver for xorg] (https://packages.ubuntu.com/bionic/xserver-xorg-video-dummy),
but I have not been able to experiment very hard with that. In addition, I am
a bit reluctant to tinker with the Xorg configuration, especially because of that
these days it is no longer explicitly needed.

A second problem is the delay with which the image is shown on the tablet
is becoming. I'm not sure if that is due to the latency of it
network, or that simply renders the tablet slow. For a terminal window
the delay is acceptable, but if you fancy something
want to run on, like Hipchat (Hipchat? Fancy? I do not understand why one
IM application needs so much time to render, but that is one
different story), then it is not that convenient.

## Conclusion

A tablet as a third screen: it works with xrandr and x11vnc, but with a few
restrictions. Try
get it out, and if you have any tips to refine this solution,
give them a go.
