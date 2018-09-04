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

At work I have a docking station, that allows me to use two
external screens with my laptop. And I find this extremely useful.
I put a terminal window here, some source doe there, a similar
source file over there, and I need to write a unit test while
looking up things on Stack Overflow. So those three screens are
easily filled.

Sometimes I work from home. I don't have a docking station at my
place, and I can use only one external screen. This works as well,
but often I miss the extra screen real estate.
We do have a tablet though, mainly used by our children.
Those are typically away or asleep when I code... So how difficult could
it be to use the tablet as an external screen?

![a tablet as external screen](/galleries/3rd_Screen/3rdscreen.jpg)

<!-- TEASER_END -->

Of course this turned out to be way more difficult than I initially thought it
would be. (A detail I might have failed to mention is that I am running
Ubuntu 18.10 on my laptop.) It took some fiddling, but in the end
I have a working system. It is not the world's most beautiful
solution, but it will do for me.

## Thank you, Mike

I found my inspiration in a blog post on
[Mike's Coding Oddities](https://mikescodeoddities.blogspot.com/2015/04/android-tablet-as-second-ubuntu-screen.html).
It needed some tinkering, because I need a 3rd screen, Mr. Mike
needs a 2nd screen.

The proposed solution does not work for Wayland. Which is a shame,
becuase Waylands sounds so much cooler than the outdated Xorg.
But if you want more screen space, you need to make sacrifices.

## My configuration

The on-board screen of my laptop is 'called' eDP-1. The external
screen is HDMI-1. I always put the external screen at the right
side of my laptop, and I want the tablet to the right of the
external screen. Both laptop and external screen have a resolution
of 1920x1080. The tablet has a screen resolution of 1920x1200.
Of which 1920x120 pixels will not be used because of the solution
I choose, but it is the best I can do for now.

(Finding the names of the outputs, eDP-1 and HDMI-1, was not easy.
I ended up installing and running [arandr](https://christian.amsuess.com/tools/arandr/) to find
out how the outputs are called, but there are probably better ways
to do this.)

## The script

This is the script I use to get my setup right:

```
#!/bin/bash

sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 3840x1080+1920+0/3840x1080+1920+0
sleep 3
sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 1920x1080+1920+0/3840x1080+1920+0
# WARNING! Unprotected server, connection not encrypted. Don't use as-is on
# a public or untrusted network!
x11vnc -clip 1920x1080+3840+0 -nocursorshape -nocursorpos
```

You need to use `sudo`. Omitting sudo does not produce error
messages, but it will not work either.

As I understand it, the first line ensures that the screen space
for my 3 screens all together becomes 5740x1080 pixels. The external screen
(HDMI-1) became twice as wide (3840x1080), and is located to the right of the
laptop screen (+1920+0).

If you only execute the first line of the script, the external screen 'slides'
as a 'view port' over the wider logical screen when you move the mouse
cursor to the left side or right side of the screen.

`sleep 3` just waits for three seconds, and the next line limits the
area of the logical screen that can be displayed on the physical screen by
1920x1080 pixels. That way, you can move your mouse cursor outside the
visible part of the screen, without having the view port sliding.

At last I start a VNC server, to serve the invisible area of the external
screen. The options `-nocursorshape` and `-nocursorpos` take care of the
mouse cursor being controlled by the VNC server (laptop), and not by the client
(tablet).

Now you need to install a VNC client on the tablet, to connect to your PC. I use
[VNC viewer](https://play.google.com/store/apps/details?id=com.realvnc.viewer.android),
but other viewers are availabe.

**Please note** that the VNC connection created this way, is not secured,
neither encrypted. That's fine for testing, or when you are in complete control
of your network. But if that is not the case, use ssl to encrypt your connection,
and put a password
on your VNC connection. (I don't know yet how to tho this, but I guess if you
look at the man page of x11vnc, you will find some hints if you search for
`-ssl` or `-rfbauth` )

## Room for improvement

This solution works readonably well, but it has its limitations. First of all,
Linux handles the external screen and the tablet as one logical screen. The
annoying consequence is that when you maximize a window, the window is
scattered over both the external screen and the tablet. To work around this
issue, you can - instead of maximizing a window - use 'dock left' and
'dock right'. Gnome has shortcut keys for this: super key + left arrow and
super key + right arrow. For other display managers, this may vary.

I think there should be a way to create a real virtual screen for the tablet
to display, so that its resolution, orientation and position can be configured
independently of the other screens. A
[dummy video driver for xorg](https://packages.ubuntu.com/bionic/xserver-xorg-video-dummy),
exists, but I have not tried yet to make that work. I am also a bit reluctant
to tinker with the Xorg configuration file, especially because these days this
is file is not explicitly needed any more. Maybe a similar solution for Wayland
exists, of which I'm not aware.

A second problem is that the display on the tablet comes with some delay. I'm
not sure whether this is due to network latency, or maybe the tablet is too
slow to render the stream. If you put a terminal window on the tablet, the
delay is acceptable, but for fancy things like Hipchat (Hipchat? Fancy? I
do not understand why an IM application can be so hard to render. But that is
a different story), it is not that convenient.

A third thing I noticed, is that the VNC server sometimes turns off the
'key repeat functionality'. I mean, if you press e.g. backspace for a long
time, it only deletes one character, instead of continuing to delete until the
key is released. To fix this, type `xset r on`. If it does not work, type it
again. And again. In the end, it will probably work. (Look at the log messages of your VNC
server to find out how many times you need to enter the command.)

## Conclusion

A tablet as a third screen: it works with xrandr and x11vnc, but with a few
restrictions. Just
try it, and if you have any tips to improve this solution,
let me know.
