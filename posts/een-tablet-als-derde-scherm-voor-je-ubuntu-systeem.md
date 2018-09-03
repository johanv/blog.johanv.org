<!--
.. title: Een tablet als derde scherm voor je Ubuntu-systeem
.. slug: een-tablet-als-derde-scherm-voor-je-ubuntu-systeem
.. date: 2018-09-01 22:22:14 UTC+02:00
.. tags: ubuntu, linux, tips
.. category:
.. link:
.. description: Gebruik een tablet als extra scherm.
.. type: text
-->

Op het werk heb ik een docking station, en op die manier hangen er twee
externe schermen aan mijn laptop. En dat vind ik echt handig om te
programmeren. Een terminalvenster hier, broncode daar, een gelijkaarig
codebestand ginder, en dan nog een unit test nog ergens anders... Drie
schermen zijn al gauw vol.

Ik werk ook al eens van thuisuit. Daar heb ik geen docking station, en
maar één extern scherm. Dat gaat ook, maar extra schermruimte is toch
altijd meegenomen. Op een dag viel mijn oog op de Android-tablet die we
thuis hebben liggen. Hoe moeilijk kon het zijn om die als extra scherm
te gebruiken?

<!--
.. TEASER_END
-->

Dat was nog niet zo makkelijk als het leek. O ja, ik vergat te vertellen
dat ik Linux gebruik als OS op mijn laptop. (Want je gebruikt ook al eens graag
docker als je wilt programmeren, en als je niet de tijd hebt om minutenlang
te wachten op iedere docker-operatie, is Linux een must. En er zijn nog
1000 andere redenen om Linux te gebruiken, maar die lees je maar eens in
mijn andere blog posts.) Ubuntu 18.10 tegenwoordig. Ik heb mijn derde
scherm aan de praat, maar het is nog niet helemaal zoals ik het zou willen.
Maar het werkt al goed genoeg om het te delen, en misschien heeft iemand wel
tips om mijn oplossing te verbeteren.

## Hoe zou het kunnen werken?

Mijn ideale oplossing zou zijn mocht je onder 'Settings', 'Displays' 3
schermen te zien krijgen, waarbij je via VNC een connectie maakt naar dat
derde scherm. Dat is niet gelukt.
Ik vermoed dat dat wel op één of andere manier moet gaan met
[xserver-xorg-video-dummy](https://packages.ubuntu.com/bionic/xserver-xorg-video-dummy),
maar ik heb daar nog niet erg hard mee kunnen experimenteren, en ik ben
wat terughoudend om met de Xorg-configuratie te prutsen, zeker omdat die
tegenwoordig niet meer expliciet nodig is.

## Hoe kreeg ik het aan de praat?

Wat wel werkte, is iets dat gebaseerd is op een blog post uit
[https://mikescodeoddities.blogspot.com/2015/04/android-tablet-as-second-ubuntu-screen.html](Mike's Coding Oddities).
Ik heb er wat aan moeten prutsen, aangezien ik een derde scherm nodig had,
en Dhr. Mike een tweede.

De voorgestelde oplossing werkt niet als je Wayland gebruikt. Wat jammer is,
want Wayland klinkt zoveel cooler dan het verouderde Xorg. Maar voor wat
extra schermruimte moet je wat over hebben

Wat wel werkte, is iets dat gebaseerd is op een blog post uit
[https://mikescodeoddities.blogspot.com/2015/04/android-tablet-as-second-ubuntu-screen.html](Mike's Coding Oddities).
Ik heb er wat aan moeten prutsen, aangezien ik een derde scherm nodig had,
en Dhr. Mike een tweede.

De voorgestelde oplossing werkt niet als je Wayland gebruikt. Wat jammer is,
want Wayland klinkt zoveel cooler dan het verouderde Xorg. Maar voor wat
extra schermruimte moet je wat over hebben. Aanloggen dus met Gnome on Xorg
(ik ben ook een van de weinige stock Gnome gebruikers op deze planeet);
Ubuntu zonder Wayland zal normaal ook werken.

## Mijn configuratie

Het scherm van mijn laptop 'heet' eDP-1, mijn externe scherm 'heet'
HDMI-1. Het externe scherm staat rechts van mijn laptop, en ik wil de tablet
rechts van het externe scherm. Zowel mijn laptop als mijn externe scherm
hebben een resolutie van 1920x1080. De tablet heeft een resolutie van
1920x1200. In praktijk zullen er van mijn tablet 1920x120 pixels ongebruikt
worden. Wat jammer is.

(Het vinden van die namen van die outputs, eDP-1 en HDMI-1, vond ik niet
vanzelfsprekend. Uiteindelijk is het me gelukt door het tooltje
arandr te installeren en te runnen.)

Dit is nu het script dat ik gebruik:

```
#!/bin/bash

sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 3840x1080+1920+0/3840x1080+1920+0
sleep 3
sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 1920x1080+1920+0/3840x1080+1920+0
x11vnc -clip 1920x1080+3840+0 -nocursorshape -nocursorpos
```

De 'sudo' is nodig. Als je die weglaat, krijg je geen foutmelding, maar werkt
het ook  niet. Sudo dus.

Ik denk dat de eerste lijn ervoor zorgt dat de schermruimte voor mijn 3
zogezegde schermen samen 5740x1080 is geworden. Mijn externe scherm (HDMI-1)
is nu 2 keer zo breed geworden (3840x1080), en bevindt zich rechts van het
scherm van de laptop (+1920+0).

Als je enkel de eerste regel zou uitvoeren,
kun je het externe scherm laten 'pannen' over de brede ruimte. Als je met
de muis naar de linkerkant of rechterkant van het scherm beweegt, 'schuift' je
viewport heen en weer over het bredere logische scherm.

Dan staat er `sleep 3`, en de regel daaronder beperkt het stuk dat zichtbaar kan
zijn op het fysieke scherm tot 1920x1080. De muis kan verder naar rechts
bewegen, maar dat zie je niet meer.

Tenslotte start ik een vnc-server, die het onzichtbare stuk van het externe
scherm beschikbaar stelt.  Met `-nocursorshape` en `-nocurorpos` geef ik
aan dat ik de cursor van de server (laptop) wil zien en gebruiken, en niet
die van de client (tablet).

Nu komt het er nog op neer om een VNC-client op de tablet te installeren.
Ik gebruik [VNC viewer](https://play.google.com/store/apps/details?id=com.realvnc.viewer.android),
maar er zijn er andere genoeg. Let wel op, die connectie is niet beveiligd
en niet geëncrypteerd. Dat is OK om even te testen, of als je volledige
controle hebt over alles wat op je netwerk gebeurt. Is dat niet het geval,
gebruik dan ssl, en zet een wachtwoord op je VNC-connectie, en gebruik SSL
voor encryptie. (Ik weet eigenlijk zelf nog niet hoe je dat doet. Maar
sla er de man-page van x11vnc eens op na, zoek naar `-ssl` en naar
`-rfbauth`.)

## Ruimte voor verbetering

Deze oplossing werkt redelijk goed, maar er zijn wat beperkingen. Allereerst
zijn je externe scherm en je tablet voor de Linuxomgeving samen één scherm.
Dat heeft als vervelend gevolg dat als je een venster maximizet, dat venster
verspreid te zien is over die twee schermen. Je kunt daar wel rondwerken
door in plaats van je venster te maximizen 'snap left' en 'snap right' te
gebruiken; de sneltoetsen daarvoor zijn superkey + pijl links en
superkey + pijl rechts.

Tweede probleem is dat niet alle schermruimte op de tablet gebruikt wordt.
Ik denk niet dat dat op te lossen is als je deze manier van werken gebruikt,
aangezien het externe scherm en de tablet hetzelfde aantal pixels moet hebben
in de breedte of in de hoogte als de tablet. Bovendien gaat
'snap left' en 'snap right' niet meer zo handig werken als de twee
resoluties verschillen.

En tenslotte een laatste probleem: het beeld op de tablet komt met wat
vertraging. Ik ben er nog niet uit of dat komt door de latency van het
netwerk, of dat de tablet gewoon te traag rendert. Als je op de tablet een
terminalvenster zet, dan is dat goed werkbaar, maar als je er iets fancy
op wil runnen, zoals Hipchat (Hipchat? Fancy? Ik snap niet waarom een
IM-toepassing zo veel tijd nodig heeft om te renderen, maar dat is een
ander verhaal), dan is het niet zo handig.

## Conclusie

Een tablet als derde scherm: dat werkt, maar met een paar issues. Probeer
het eens uit, en mocht je nog tips hebben om deze oplossing te verfijnen,
laat dan zeker iets weten.
