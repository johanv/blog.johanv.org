<!--
.. title: Getting rid of Google
.. slug: getting-rid-of-google
.. date: 2018-11-07 21:26:29 UTC+01:00
.. tags: google, grog, privacy, android
.. category: 
.. link: 
.. description: Leaving Google is not easy. Today I'll take a first step.
.. type: text
-->

I've never trusted Google less than today. Yet Google has never known me
better. Google knows my contacts and my calendar. Google knows where I am, it
has access to all my photos, and it reads lots of the emails I receive. Google
used to see my DNS requests, and I would not be suprised if Google has my finger
prints.

That is way too much information, this should come to an end. But since I don't
want to start over from scratch, I want to do leave Google taking one step at
the time. I will start today.

![getting rid of google](/galleries/grog/grog.png)

<!-- TEASER_END -->

## Step 1: Lineage OS

I've got a Google Nexust 5X phone, and this way I more or less sold my soul to the company.
Leaving Google will not be easy, and I should take one step at the time, because I don't
want to break all my workflows at once. It will take time, and I probably will not be able
to break up with Google completely. But I have to start somewhere, and as a first step, I
will replace the OS on my phone. By [LineageOs](https://lineageos.org), a version of
Android without Google. [I've done this before](/categories/cyanogenmod), and it never
caused too much trouble.

Installing a new ROM is always a little tricky. You get warnings all over the place about
voiding your warranty, and about how irreversible the process is. Moreover, you install
some OS and some bootloader you are downloading from the Internet; you just have to trust
those will not do evil things. But since I assumed those images are at least as trustworthy
as Google, I just flashed them.

The installation of LineageOS was not that hard. The [wiki](https://wiki.lineageos.org)
has step by step instructions, and the tools you need (adb and fastboot) are in Ubuntu's
repositories. The hardest part was getting my phone in developer mode, which involved
touching Android version number (or build number, I don't remember exactly) multiple times.

## The app store

The instructions on the LineageOS wiki mentioned how I could optionally install the Google
Apps. For me this was not optional. Today I use a whole lot of apps that are only available
via the Google Play store. Probably there are some hacky ways to get those apps without
having to log in with your Google accounts, but I considered that this would be a little
too experimental for a first step. Moreover my calendar and my contacts are still stored on
Google's servers. Moving those over to something else will be something I will do later on.

So after I booted into the new OS (the first boot after flashing always takes a long time,
no worries there), I entered my Google credentials. And I activated finger print
authentication. Which is so easy. I figured that if Google stores the fingerprints on
its servers, it already has mine. I installed WhatsApp as well, which is a Facebook product.
Facebook is also a company that has too much information on me, but at the moment I just
have to use it because lots of my contacts do.

I installed Firefox as well; Mozilla is a company that I still trust. I changed Firefox's
default search engine; my search queries now go to [DuckDuckGo](https://duckduckgo.com)
instead of to Google.

Some other proprietary apps I use, are KBC Mobile (for paying things I buy online),
Payconiq (for paying beer), and Itsme (for authenticating on the government's
applications). I was happy to see that those did not complain about ithe
&lsquo;unofficial&rsquo; operating
system. Even Telenet Yelo Play, which refused to work on a device I flashed with
CyanogenMod some years ago, is now running fine.

I also installed PocketCasts, which I use as podcatcher. But I think I will be able to
replace this app by an alternative one from [F-Droid](https://f-droid.org). That will
probably be one of my next steps. The lesser apps that I need to download from the Google
Play Store, the better.

## What's next?

This concludes the frist step I took in my journey away from Google. I hope more steps are
to follow soon. If so, I will blog about it. If not, [getting rid of
google](/categories/grog/) wil be added to my list of ambitious projects that didn't go
anhywhere.

## Thank you, Paul

The evolution of Google from a cool company to an evil company, is not new for me. I've
thought about being less dependent of Google multiple times, but I never took some actual
action.

But now that [Paul Thurrott](https://www.thurrott.com/) was recently complaining about the
trustworthyness of Google on [Windows Weekly](https://twit.tv/shows/windows-weekly), I
decided to actually start &lsquo;degoogling&rsquo;. Pauls blog posts
[Android without
Google](https://www.thurrott.com/mobile/android/190626/android-without-google) and
[Trusting Google](https://www.thurrott.com/google/189778/trusting-google) are definitely
worth reading.

