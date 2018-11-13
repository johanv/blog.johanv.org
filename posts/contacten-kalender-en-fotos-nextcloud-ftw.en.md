<!--
.. title: Contacts, calendar and photos: NextCloud FTW
.. slug: contacten-kalender-en-fotos-nextcloud-ftw
.. date: 2018-11-13 21:21:57 UTC+01:00
.. tags: google, grog, privacy
.. category: 
.. link: 
.. description: 
.. type: text
-->

I've never had a mobile phone before 2010. I used to say that if people wanted
to contact me, it should take some effort. So when I bought my first mobile
phone back in 2010, I wasn't really interested in having a phone; I was looking
for a convenient portable computing device. So I got myself a smartphone.

I remember that I really enjoyed that the phone numbers of my contacts were
stored in my Google account. This way I did not have to input them again after e.g.
flashing a new ROM. (And at once I could get rid of the hand written list I copied every
year.)

But this convenience comes with a price. Google knows all my contacts, and their
phone numbers. Let's put this to an end.

![Apps that have permissions to access my contacts](/galleries/grog/toegang-contacten.png)

As from today, [NextCloud](https://nextcloud.com) will be the one that keeps my
contacts, calendar and photos.

<!-- TEASER_END -->

(This blog post is part of my &lsquo;[getting rid of
Google](/en/categories/grog/)&rsquo;-series.)

## NextCloud

NextCloud is often used to keep documents on some computer on the Internet, so
that you can easily access those documents from all of your PC's, tablets and
phones. This makes it comparable to DropBox, Google Drive and Microsoft
OneDrive. Just like Google and Microsoft, NextCloud can also store your contacts
and your calendar. Just what I need for today's case.

## Hosted NextCloud

Nextcloud is free software, licensed under the [GNU
AGPL license](https://en.wikipedia.org/wiki/GNU_Affero_General_Public_License). 
This implies that I can install the software on one of my computers, without
having to pay. Which is very nice, but then I would need a server that is always
online. A server that needs maintenance, that has to be secured, and always has
updates waiting to be installed. I don't have time for this, so I went looking
for a hoster who can do all this for me.

I decided to go with [YourOwnNet](https://yourownnet.net/nextcloud/). 
I am not related in any way to this company, but their website told that they
have NextCloud hosting from 5 EUR/year, which is affordable. So instead of
giving all my info to Google, I will give it to YourOwnNet.

That might not be
ideal either, but I think it is less problematic. My NextCloud-instance on
YourOwnNet does not show ads, so it wil not show personalized ads either. Moreover most of
my contacts won't have a YourOwnNet-account, so YourOwnNet will have a harder
time to link information to the relevant person.

(I think NextCloud allows storing the data encrypted on the server as well, but
I'm not sure I can do this for the cheap formula of 5 EUR/year.)

I contacted YourOwnNet, I asked for an account, and now I have 7 days to try it
out for free.

## Contacts and calendar

To migrate my contacts and calendar from Google to NextCloud, I just exported
the information from Google (which was easier than I thought it would be), to
import it into NextCloud.

To export your Google contacts, surf to
[contacts.google.com](https://contacts.google.com), click &lsquo;More&rsquo;,
&lsquo;Export&rsquo;. You are asked for a format for the export; I chose
&lsquo;vCard (for iOS-contacts)&rsquo;. (iOS? Never heard of that one ;-))
Then go to the contacts-page in your NextCloud-instance, click
&lsquo;Settings&rsquo; at the bottom left, and then &lsquo;Import to
Contacts&rsquo;. Upload your file, and you're done.

For calendar items, the procedure is similar. In the
[Google Calendar web interface](https://calendar.google.com), click on the
gear-icon in the top right corner, and then &lsquo;Settings&rsquo;. You are
directed to another page, where you can click &lsquo;Import and export&rsquo;,
and then &lsquo;Export&rsquo;. This downloads a zip-file containing a file for
each calender you access via Google.

Unpack the zip-file, and upload your calendar file from the calendar page on
NextCloud.

## Using NextCloud for your contacts and calendar on your Android device

Via [F-Droid](/posts/apps-f-droid-t-the-rescue/) I installed the 
[NextCloud Android app](https://github.com/nextcloud/android)
and [DAVdroid](https://f-droid.org/en/packages/at.bitfire.davdroid/). 
Using the NextCloud app you can log on to your NextCloud instance. Then use the
&lsquo;hamburger menu&rsquo; (three horizontal lines) to go to
&lsquo;settings&rsquo; and &lsquo;Synchronize calendar &amp; contacts&rsquo;.
This way you link your device to your NextCloud instance.

If DAVdroid asks you a question about contact groups (I don't remember exactly), 
[choose &lsquo;per-contact
categories&rsquo;](https://www.davdroid.com/tested-with/nextcloud/#c16).

Then you have to tell your contacts app on your device that you want to see your
NextCloud contacts instead of the Google contacts. This is done via the
hamburger menu of the contacts app.

![Contacten van NextCloud gebruiken](/galleries/grog/contacten-hamburger.png)

If you go to your calendar app, you will now see that a new calendar is listed: the
one you just uploaded to NextCloud. Enable the new one, and hide your old Google 
calendar from your phone.

## Sharing calendars

I still have to find out how you can conveniently share your calendar with
someone else. I might write a blog post about this later on.

You should be aware that if you share your calendar with someone that is using
Google, Google will still be able to see all your appointments. But at least they'll have
less control.

## Backing up photos to NextCloud

After I had [flashed Lineage OS on my phone](/en/posts/getting-rid-of-google/)
some days ago,
I didn't reinstall Google Photos. Because my photos will now be uploaded to
NextCloud instead of to Google. You can enable the synchronisation of your
photos using the NextCloud app:
hamburger, &lsquo;Auto upload&lsquo;, and then tick the cloud for your
Camera-folder.

## Back off, Google!

So now I don't need Google for my contacts, calendar and photos, so let's revoke
some of the permissions of Google.

Via the settings of your phone, you can go to &lsquo;Apps and
notifications&rsquo;, and then to &lsquo;App permissions&rsquo;. Here you can
see for each resource to which apps it is available. It seemed that Google and
Google Play Services had access to my calendar, my contact, my location, my
microphone, my messages and my phone. I turned those off one by one. Google was
threatening me that things might stop working, but I don't really believe that.
I haven't noticed any problems for the time being.

## Find my phone

I guess that now I've denied Google the access to my location, Google will
probably not find my phone when I have forgotten where I left it.
So I searched for
another alternative; I might have found something: a combination of
[PhoneTrack](https://apps.nextcloud.com/apps/phonetrack) (a NextCloud-app
I had to update) and
[μlogger](https://github.com/bfabiszewski/ulogger-android) (app available in 
F-Droid). I will blog about the details later.

## That will do for today

Here you are. Once again Google has
[less access to my personal life](/en/categories/grog/).

Theoretically I can now remove my contacts and my calendar items from my Google
account. But I will leave them for the time being; Google already has that
information anyway. (And my NextCloud instance is not completely set yet, I am
still in the trial period.) However, I will not update my contact info on Google
anymore.

The same for my photos on Google Photos. They stay where they are; Google has
them already. I have not the intention yet to move all my photos to nextcloud.
