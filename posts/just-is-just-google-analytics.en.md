<!--
.. title: Put your money where your mouth is: Google Analytics
.. slug: put-your-money-where-your-mouth-is-google-analytics
.. date: 2018-11-20 22:48:40 UTC+01:00
.. tags: google, grog, privacy
.. category: 
.. link: 
.. description: 
.. type: text
-->

All of a sudden I saw the light. In this series I evangelize that Google is
evil, and how I don't want to be tracked. But meanwhile I secretly tell Google
that you are reading my blog, via Google Analytics. Which is at least a bit
hypocritic.

I don't even remember why I ever put the Google Analytics script on mysite.
Sometimes I looked to the list of popular blog posts, but I can do that without
Google Analytics. So let's dump it.

![Google Analytics in the garbage can](/galleries/grog/analytics-vuilnis.png)

[Removing Google Analytics
from my site](https://github.com/johanv/blog.johanv.org/commit/c6b5c49ef853984356cea989fac3c1d093bb7098)
is nothing more than deleting a script. And with those 10 lines of Javascript, I
could delete half of my
[privacy statement](/en/pages/privacy) as well. Win-win.

<!-- TEASER_END -->

Meanwhile I use
[Webalizer](http://www.webalizer.org/) again to gather statistics about page
visits on my blog, like in the old days. I suspect Webalizer has not
changed since I used it the last time; it's latest release was in 2012.
But it still does what it is supposed to do.

By looking at the Webalizer stats, I discovered that an old post I wrote back in
2012 about a
[Raspberry Pi print server](/posts/old/node-195), is still rather popular. On
this page, I use a monotype-font for file names and commands, and it seemed that
the combination of this monotype font with the default font of the
[hack theme](https://themes.getnikola.com/v8/hack/), which I was using until recently, was very
ugly.

So I returned to a more default theme for my blog, maybe I will personalize the
theme again at some point in the future.

But meanwhile Google can't track you via the font of the hack theme. So
everybody is happy now.
