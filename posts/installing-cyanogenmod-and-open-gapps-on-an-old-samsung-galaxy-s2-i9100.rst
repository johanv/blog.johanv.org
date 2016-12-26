.. title: Installing CyanogenMod and Open Gapps on an old Samsung Galaxy S2 i9100
.. slug: installing-cyanogenmod-and-open-gapps-on-an-old-samsung-galaxy-s2-i9100
.. date: 2016-12-26 15:40:05 UTC+01:00
.. tags: cyanogenmod,android,samsung,hacks
.. category:
.. link:
.. description:
.. type: text

Last week, I dropped my OnePlus One phone. The touch screen broke, and stopped
working. Eventually, I will buy a new phone, but for the time being, my
father gave me his old Samsung Galaxy S2 i9100. It was running Android
2.something, so I decided to upgrade it to Cyanogenmod 13 (which uses
Android 6.0).

I had some troubles with using the Cyanogenmod wiki, so I used googles cache.
It took me more than a day to realize that `Cyanogenmod seems to have ceised
existing
<https://www.xda-developers.com/the-death-of-cyangenmod-and-whats-in-store-for-the-future/>`_.
So I guess this was the last time I installed CyanogenMod. But nevertheless,
let's document how I did it, because it was way more difficult than I expected
it to be.

I am using Fedora 24. I think I installed adb as follows:

.. code-block::

    yum install android-tools

The installation of CyanogenMod 13 itself is well documented in
`the wiki <https://web.archive.org/web/20161224194651/https://wiki.cyanogenmod.org/w/Install_CM_for_i9100>`_,
but I had some troubles using heimdall to flash a new recovery image. The error
message was:

.. code-block::

  Partition "kernel" does not exist in the specified PIT.

I was using the 'zImage' from the tar-file I downloaded using the `direct
download link <https://web.archive.org/web/20161224194651/https://www.androidfilehost.com/?fid=95916177934516900>`_ on the wiki page.

What I had to do, was unplug the device, reboot it into download mode, plug it
in again and then I had to enter:

.. code-block::

    heimdall flash --KERNEL zImage --no-reboot

So KERNEL should be in all caps, but it was really necessary to unplug,
reboot, and replug. Which I found out after way to many tries.

When the new kernel/recovery was installed, I booted into recovery, and I
was able to install the latest nightly (ever as it seems) using
``adb sideload``.

But then I wanted to install the Google Apps, and even though I downloaded
the 'pico' version of `Open GApps <http://opengapps.org/?api=6.0&variant=pico>`_,
my device complained about not enough space availabe. (Error 70 or something.)
So I had to repartition the device, in order to increase the size of the
system partition.

I needed a Windows PC to accomplish that, and I followed more or less what's on
`this thread on XDA-developers <http://forum.xda-developers.com/galaxy-s2/development-derivatives/mod-increase-partition-size-t3011162>`_.

I downloaded Odin v3.07 from this very page, and the zip with pit-files as
well. I don't like it at all to download software from a random forum thread,
but I really got desparate, and I was happy it eventually worked. I needed
to install the drivers for the phone as well, but those I downloaded from
`samsungdrivers.net <http://www.samsungdrivers.net/samsung-galaxy-s-ii-software/>`_.
(Not sure that's an official site, but some
firewall blocked the zip-file with the drivers from the forum posts.)

Now if I remember it correctly, I used Odin with the
pit file ``I9100_1GB-System_3GB-Data_512MB-Preload.pit`` file to
change the partitions on my phone. To be fair, I was just
guessing. **I didn't flash a kernel using odin**, because I
got an error message.

So after repartitioning, I re-flashed the recovery image with
heimdall, I reformatted the internal sd card in recovery mode,
and then I sideloaded CyanogenMod again, and the pico-image
of OpenGapps.

And it worked. For the last time. I guess I'll keep an eye
on `Lineage OS <http://lineageos.org/>`_ and
`Dirty Unicorns <http://dirtyunicorns.com/>`_.
