<!--
.. title: Getting rid of Google
.. slug: getting-rid-of-google
.. date: 2018-11-05 21:26:29 UTC+01:00
.. tags: google, grog, privacy, android
.. category: 
.. link: 
.. description: Google verlaten is niet makkelijk. Vandaag zet ik de eerste stap.
.. type: text
-->

Mijn vertrouwen in Google staat historisch laag. Maar toch weet Google meer van
mij dan ooit tevoren. Google kent mijn contacten, en mijn agenda. Google weet
waar ik ben, heeft al mijn foto's, en leest nog steeds veel e-mails die
voor mij bedoeld zijn. Google volgde ooit al mijn DNS-requests,
en het zou me niet verbazen mocht Google ook mijn vingerafdrukken kennen.

Google heeft toegang tot veel te veel informatie over mij, en dat moet maar
eens stoppen. Dat gaat mijn voornemen zijn voor 2019. Maar aangezien ik niet
meteen alles overboord wil gooien, zal ik dat in stappen moeten doen. Dus kan
ik er nu al best aan beginnen.

![getting rid of google](/galleries/grog/grog.png)

<!-- TEASER_END -->

## Stap 1: Lineage OS

Ik heb een Google Nexus 5X-telefoon, en op die manier verkocht ik bij wijze van
spreken mijn ziel. Mijn afscheid van Google zal dus niet gemakkelijk zijn,
en stap voor stap
moeten gebeuren, want ik wil niet al mijn workflows tegelijk breken. 
Ik vrees ook dat het niet snel tot een
definitieve scheiding zal komen. Maar ik moet ergens
beginnen, en stap 1 is geworden: het besturingssysteem van mijn telefoon
vervangen. Door [LineageOs](https://lineageos.org), een versie van Android, maar
dan zonder Google. Ik had zoiets [vroeger al meer
gedaan](/categories/cyanogenmod/), en dat is toen ook 
altijd vrij makkelijk gelukt.

Het installeren van een nieuwe ROM is altijd wat spannend. Zeker omdat je
waarschuwingen krijgt over je garantie die je gaat verliezen, en over hoe
onomkeerbaar het proces wel is. Bovendien installeer je een OS en een
bootloader die je ergens downloadt, en waarvan je dan maar moet vertrouwen dat
ze niet schadelijk zijn. Ik ging er maar van uit dat die images minstens even
betrouwbaar waren dan Google, dus heb ik ze maar geflasht.

Dat ging als vanouds behoorlijk vlot. De [wiki van
LineageOS](https://wiki.lineageos.org) legt stap voor stap uit wat je moet
doen, en de tools die je nodig hebt (adb en fastboot) zitten gewoon in de
repository's van Ubuntu. De enige moeilijkheid was om mijn telefoon in
developer-modus te krijgen. Daarvoor moest ik via de instellingen naar 'over de
telefoon', en daar een tiental keer op het Android-versienummer 
(of build-nummer, ik weet het niet meer precies) tikken.

## De app store

In het stappenplan op de wiki van LineageOS stond ook dat ik optioneel de
Google Apps kon flashen. Voor mij is dat niet optioneel. Ik gebruik vandaag nog
heel wat apps die ik via de Google Play Store installeer. Er bestaan allicht wel
wat hacks om get zonder te doen, maar dat vond ik toch nog wat experimenteel
voor een eerste stap. Bovendien zitten mijn agenda en al mijn contacten nog bij
Google. Die moet ik nog verhuizen naar ergens anders, maar dat is voor later.

Dus bij mijn eerste reboot in het nieuwe OS (die eerste reboot duurt altijd lang, geen
paniek. Latere reboots gaan altijd sneller), gaf ik braaf mijn Google-credentials
in. Ik activeerde ondanks alles het aanmelden met mijn vingerafdruk (want als
Google de vingerafdrukken bewaart, dan heeft het ze toch al), en installeerde
WhatsApp. WhatsApp is van Facebook, nog zo'n evil company die al veel te veel
over mij weet. Maar ik zit nog met het probleem dat te veel van mijn contacten
WhatsApp gebruiken om me te contacteren.

Ik installeerde ook Firefox, want Mozilla is een bedrijf dat ik tot nader order
wel vertrouw. Voorlopig nog zonder Firefox sync. En ik veranderde de
standaardzoekmachine, zodat mijn zoekopdrachten nu in eerste instantie naar
[DuckDuckGo](https://duckduckgo.com) gaan in plaats van naar Google.

Andere toepassingen die ik gebruik zijn KBC Mobile (handig, online aankopen
betalen met de telefoon), Payconiq (handig, pinten betalen met de telefoon), en
Itsme (handig, authenticeren bij de overheid met de telefoon). Tot mijn vreugde
deden die apps er niet moeilijk over dat ik een custom firmware gebruik. Zelfs de
evil Telenet Yelo Play app valt niet meer over LineageOS, dat was vroeger zeker
anders.

Verder installeerde ik PocketCasts, omdat dat momenteel nog mijn standaard
podcatcher is. Nu denk ik wel dat ik voor die toepassing een alternatieve kan vinden
in [F-Droid](https://f-droid.org). Dat gaat allicht een van mijn volgende stappen
zijn. Hoe minder apps die per definitie uit Googles app store komen, hoe beter.

## What's next?

Tot zover de eerste stap in mijn lange toch weg van Google. Hopelijk volgen er
nog. Zo ja, zal ik erover bloggen. Zo nee, zal [getting rid of
google](/categories/grog/) bij op mijn lijstje prijken van
ambitieuze projecten waar uiteindelijk niets van terecht kwam.

## Merci, Paul

Dat Google de laatste jaren in sneltempo evolueerde van een coole naar een
kwaadaardige onderneming, is niet nieuw. Ik had een aantal jaren geleden al een
deel van mijn mail ontgoogled, maar dat was dan blijven liggen. Het is omdat
[Paul Thurrott](https://www.thurrott.com/) zich onlangs in [Windows
Weekly](https://twit.tv/shows/windows-weekly) druk maakte over Google, dat ik de
draard van het ontgooglen terug opnam. Zij blog posts
[Android without
Google](https://www.thurrott.com/mobile/android/190626/android-without-google) en
[Trusting Google](https://www.thurrott.com/google/189778/trusting-google) zijn
zeker het lezen waard.

