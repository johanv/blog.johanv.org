<!--
.. title: Contacten, kalender en foto's: NextCloud FTW
.. slug: contacten-kalender-en-fotos-nextcloud-ftw
.. date: 2018-11-12 21:21:57 UTC+01:00
.. tags: google, grog, privacy
.. category: 
.. link: 
.. description: 
.. type: text
-->

Ik heb heel lang zonder mobiele telefoon geleefd. Mijn motto was: &lsquo;Als je mij wilt
bereiken, moet je er moeite voor doen&rsquo;. Toen ik in 2010 voor het eerst een GSM
kocht, was ik dan ook meer op zoek naar een handig draagbaar computertje dan naar iets om
effectief mee te bellen. En zo kocht ik mijn eerste smartphone.

Wat ik toendertijd enorm handig en makkelijk vond, was dat de telefoonnummers van mijn
contacten bewaard werden in mijn Google-account. Zo had ik die telefoonnummers nog steeds
bij de hand als ik eens een custom ROM op mijn telefoon flashte.
Meteen was ik ook
verlost van het lijstje telefoonnummers dat ik jaarlijks opnieuw kopieerde (met
een echte balpen) in mijn nieuwe Chiro-zakagenda.

Uiteraard is er een keerzijde aan die medaille. Google kent nu al mijn contacten, en ook
hun telefoonnummers. Tijd dat daar verandering in komt. 

![Apps that have access to my contacts](/galleries/grog/toegang-contacten.png)

Vandaag draag ik het beheer van mijn contacten, agenda en foto's over
aan [NextCloud](https://nextcloud.com).


<!-- TEASER_END -->

(Deze blog post maakt deel uit van de reeks &lsquo;[getting rid of
Google](/categories/grog/)&rsquo;.)

## NextCloud

NextCloud is software die vaak gebruikt wordt om documenten te bewaren op een
computer op het internet, zodat die documenten toegankelijk worden vanop al je
PC's, tablets en telefoons. Op die manier is het vergelijkbaar
DropBox, Google Drive en Microsoft
OneDrive. Net zoals Google en Microsoft biedt NextCloud ook de mogelijkheid 
om je agenda en je contacten
te bewaren. En dat is wat ik vandaag ga doen.

## Hosted NextCloud

Nextcloud is vrije software, onder de [GNU
AGPL-licentie](https://nl.wikipedia.org/wiki/Affero_General_Public_License). 
Dat wil vanalles
zeggen, onder andere dat je die software op een eigen computer mag installeren zonder
te moeten betalen. Dat is fijn, maar dan heb je wel een server nodig die continu aan het
Internet hangt. Zo'n server moet je onderhouden, je moet erop letten dat de beveiliging in
orde is, en je hebt altijd wel werk aan het up-to-date houden van de software.
Zoveel tijd heb ik momenteel niet, dus ik ging op zoek naar een hoster die dat in mijn 
plaats kan doen.

Ik vond [YourOwnNet](https://yourownnet.net/nextcloud/). Ik heb geen speciale banden met
dat bedrijf, maar ik zag op hun website dat ze al NextCloud-hosting aanbieden vanaf 5 EUR
per jaar. Dat past binnen mijn budget.

Als ik straks de gegevens van mijn contacten ga uploaden naar YourOwnNet, dan deel ik
ze uiteraard ook met een firma. Maar ik denk dat dat minder kwaad kan dan ze te delen
met Google. Ten eerste toont YourOwnNet me geen reclame, dus ze zullen mijn gegevens
alvast niet gebruiken om me gericht reclame te sturen. En ten tweede zullen de meeste van
mijn contacten geen account hebben bij YourOwnNet, waardoor YourOwnNet minder makkelijk
informatie aan elkaar kan linken. (Dat ligt bij Google al heel anders.)

Ik vroeg een account aan bij YourOwnNet, en ik mag alvast 7 dagen gratis
proberen.

## Contacten en agenda

Om mijn contacten en mijn agenda over te zetten van Google naar NextCloud, vond ik er
niet beter op om ze gewoon te exporteren vanuit Google (dat ging vlotter dan ik
had verwacht), en dan opnieuw te importeren in NextCloud.

Om je contacten vanuit Google te exporteren, surf je naar 
[contacts.google.com](https://contacts.google.com), en je klikt onder &lsquo;Meer&rsquo; op 
&lsquo;Exporteren&rsquo;.
Google vraagt dan in welk formaat je de contacten wilt; ik koos
&lsquo;vCard (voor iOS-contacten)&rsquo; (iOS? WTF is dat? ;-)) In NextCloud klik je
op de contactenpagina, linksonderaan klik je op &lsquo;Settings&rsquo;, en dan staat er
&lsquo;Import into Contacts&rsquo;. Bestandje uploaden, en klaar.

Voor je kalender is het gelijkaardig. In de 
[webinterface van Google Calendar](https://calendar.google.com) klik je rechtsbovenaan
op het tandwieltje, en vervolgens op &lsquo;Instellingen&rsquo;. Op de pagina waar je dan
terechtkomt, klik je op &lsquo;Importeren en exporteren&rsquo;, en vervolgens op
&lsquo;Exporteren&rsquo;. Zo download je een zip-bestand met daarin de gegevens van alle
kalenders waarvoor je toegang hebt via Google.

Dat zip-bestand pak je uit, en daarna kun je dan je eigen kalender uploaden via de
kalenderpagina op NextCloud.

## De NextCloud-contacten en -agenda gebruiken op je Androidtoestel

Ik installeerde via [F-Droid](/posts/apps-f-droid-t-the-rescue/) de 
[NextCloud Android app](https://github.com/nextcloud/android)
en [DAVdroid](https://f-droid.org/en/packages/at.bitfire.davdroid/). 
Met de NextCloud-app kun je aanloggen bij je NextCloud-instance,
en dan kun je daar via het &lsquo;hamburgermenu&rsquo; (dat zijn de drie
horizontale streepjes), &lsquo;instellingen&rsquo; en &lsquo;Synchroniseren agenda &amp;
contactpersonen&rsquo; je Androidtoestel aan je NextCloud-instantie koppelen.
Als je de vraag krijgt in verband met contactgroepen (ik herinner me niet meer
precies welke het was), [kies dan &lsquo;per-contact
categories&rsquo;](https://www.davdroid.com/tested-with/nextcloud/#c16).

Stel vervolgens in je contactenapp van je Androidtoestel in dat je de contacten
uit je NextCloud-account wilt zien in plaats van die van je Google-account. Dat
doe je via het hamburgermenu van de contactenapp.

![Contacten van NextCloud gebruiken](/galleries/grog/contacten-hamburger.png)

In je kalender-app zul je zien dat er een extra kalender bijgekomen is, namelijk
je kalender van NextCloud. Je kunt dus je persoonlijke kalender van Google
uitschakelen.

## Kalenders delen

Hoe je een NextCloud-kalender nu het makkelijkst deelt met iemand anders,
moet ik nog eens uitzoeken. Daar post ik later wellicht nog iets over.

Van zodra ik mijn kalender deel met iemand die Google calendar gebruikt, gaat
Google mijn kalender weer kennen, uiteraard. Maar dan hebben ze er alleszins wat
minder controle over dan daarvoor.

## Foto's uploaden naar NextCloud

Ik had [na het flashen van mijn OS](/posts/getting-rid-of-google/) Google Photos
niet meer geïnstalleerd, want ik zal mijn foto's vanaf nu ook uploaden naar
NextCloud, in plaats van naar Google. Dat kan ook met de NextCloud app:
hamburger, &lsquo;Automatisch uploaden&rsquo;, en dan het wolkje bij
&lsquo;Camera&rsquo; aantikken.

## En nu is het genoeg

Nu ik Google niet meer nodig heb voor het beheer van mijn contacten, kalender en
foto's, heeft Google daar ook geen uitstaans meer mee. Tijd om perissies af te
pakken.

Als je vanuit de instellingen van je telefoon op &lsquo;Apps en
meldingen&rsquo;, &lsquo;App-machtigingen&rsquo; tikt, dan kun je per resource
nakijken welke apps toegang hebben. Ik merkte dat Google en Google Services
standaard toegang had tot mijn agenda, mijn contacten, mijn locatie, mijn
microfoon, mijn SMS'en en mijn telefoon. Dat leek me toch wat veel van het
goede. Ik heb die permissies allemaal ingetrokken. Google dreigde er wel mee dat
een aantal dingen mogelijk niet meer correct zouden werken, maar ik geloof daar
niet veel van, we zullen dat wel zien.

## Find my phone

Nu vermoed ik wel dat als ik Google geen toegang meer geef tot mijn locatie,
Google mijn telefoon ook niet meer kan vinden als ik weer eens vergeet waar ik
hem heb laten liggen. Dat is een feature die ik waarschijnlijk wel ga missen,
dus daarvoor heb ik nog een alternatief gezocht. En gevonden in een combinatie
van [PhoneTrack](https://apps.nextcloud.com/apps/phonetrack) (die NextCloud-app
heb ik wel moeten updaten) en
[μlogger](https://github.com/bfabiszewski/ulogger-android) (app verkrijgbaar via
F-Droid). De details daarvan beschrijf ik nog in een latere blog post.

## Dat zal het zijn voor vandaag

Ziezo, Google kan zijn neus [weer wat minder](/categories/grog/) in mijn persoonlijk leven
steken;
in principe kan ik nu de contacten en de agenda-items bij Google
verwijderen. Maar ik ga ze voorlopig wel laten staan: Google kent ze toch al.
(En mijn NextCloud-account is nog niet helemaal in orde, ik zit nog in de
trial-periode.) Bijwerken doe ik alleszins niet meer.

Mijn oude foto's staan voorlopig ook nog bij Google Photos. Dezelfde reden:
Google heeft ze toch al. En voorlopig staan ze daar goed, ik heb niet de
intentie om al mijn Google photo's naar NextCloud te verhuizen.
