<!--
.. title: Dikdikdik: a Wiezen (Solo Whist) score app
.. slug: wiezen-score-app
.. date: 2020-03-23 21:38:00 UTC+01:00
.. tags:
.. category: wiezen,app,php,eventsourcing,whist,dikdikdik
.. link:
.. description:
.. type: text
-->

For some time I've been working on a web application that
helps you keeping track of the score if you want to play
the Wiezen card game. Wiezen, as we play it, is called
[solo whist](https://en.wikipedia.org/wiki/Solo_whist) on
Wikipedia.

Yesterday this [score application](https://www.rijkvanafdronk.be/app)
(in Dutch, sorry) reached version 1.0. So I thought it would
be appropriate to announce this release on my blog.

Admittedly, the user interface leaves a lot to be desired. But 
it turned out a very cool application (imho), with a nice
event sourced backend, and gitlab pipelines that automatically
test new source code. 🤓

The score app follows the
[whist game rules](https://www.rijkvanafdronk.be/spelregels)
as set by
[Rijk van Afdronk vzw](https://www.rijkvanafdronk.be).
The application can be tested (as long as my vps doesn't die) at
[score.rijkvanafdronk.be](https://score.rijkvanafdronk.be); source
code can be found [on gitlab](https://gitlab.com/rva-vzw/dikdikdik).

![can only get this once!](/galleries/misc/ikanemorenekerale.jpg)
