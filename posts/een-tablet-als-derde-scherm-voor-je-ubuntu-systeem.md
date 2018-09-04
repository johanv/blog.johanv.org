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

Op het werk beschik ik over een docking station, en op die manier hangen er twee
externe schermen aan mijn laptop. dat vind ik nu eens buitengewoon handig.
Een terminalvenster hier, broncode daar, een gelijkaarig
codebestand ginder, een unit test nog ergens anders, en dan misschien nog
een browser ook... De drie schermen zijn al gauw goed gevuld.

Ik werk ook al eens van thuisuit. Daar heb ik geen docking station, en
maar één extern scherm. Dat gaat ook, maar ik mis dat toch gauw, die extra
ruimte.
Op een dag viel mijn oog op de tablet die we
thuis hebben liggen. Hoe moeilijk kon het zijn om die als extra scherm
te gebruiken?

![een tablet als derde scherm](/galleries/3rd_Screen/3rdscreen.jpg)

<!-- TEASER_END -->

Uiteraard was dat toch wat moeilijker dan ik in eerste instantie dacht. O ja,
ik vergat te vertellen
dat ik Linux gebruik als OS op mijn laptop. Tegenwoordig Ubuntu 18.10, want
Ubuntu is de Linuxdistributie men gebruikt waar ik werk. Na veel proberen en
prutsen kreeg ik het derde scherm aan de praat. Het is niet de mooiste
oplossing, maar het werkt.

## Dank je wel, Mike

Ik baseerde me op een blog post uit
[Mike's Coding Oddities](https://mikescodeoddities.blogspot.com/2015/04/android-tablet-as-second-ubuntu-screen.html).
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
worden. Jammer. Maar helaas.

(Het vinden van die namen van die outputs, eDP-1 en HDMI-1, vond ik niet
vanzelfsprekend. Uiteindelijk is het me gelukt door het tooltje
[arandr](https://christian.amsuess.com/tools/arandr/) te installeren
en te runnen.)

## Het script

Ik gebruik nu het volgende script om mijn derde 'scherm' te activeren:

```
#!/bin/bash

sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 3840x1080+1920+0/3840x1080+1920+0
sleep 3
sudo xrandr --fb 5760x1080 --output HDMI-1 --panning 1920x1080+1920+0/3840x1080+1920+0
# OPGELET! Onbeveiligde server, ongeencrypteerde verbinding. Doe dit niet
# op deze manier op een publiek of onbetrouwbaar netwerk.
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

Tenslotte start ik een VNC-server, die het onzichtbare stuk van het externe
scherm beschikbaar stelt.  Met `-nocursorshape` en `-nocursorpos` geef ik
aan dat ik de cursor van de server (laptop) wil zien en gebruiken, en niet
die van de client (tablet).

Nu komt het er nog op neer om een VNC-client op de tablet te installeren.
Ik gebruik
[VNC viewer](https://play.google.com/store/apps/details?id=com.realvnc.viewer.android),
maar er zijn er andere genoeg.

**Let wel op**, de VNC-connectie is niet beveiligd
en niet geëncrypteerd. Dat is OK om even te testen, of als je volledige
controle hebt over alles wat op je netwerk gebeurt. Is dat niet het geval,
gebruik dan ssl voor encryptie, en zet een wachtwoord op je VNC-connectie.
(Ik weet eigenlijk zelf nog niet hoe je dat doet. Maar
sla er de man-page van x11vnc eens op na, zoek naar `-ssl` en naar
`-rfbauth`.)

## Ruimte voor verbetering

Deze oplossing werkt redelijk goed, maar ze heeft haar beperkingen. Allereerst
zijn je externe scherm en je tablet voor de Linuxomgeving samen één scherm.
Dat heeft als vervelend gevolg dat als je een venster maximizet, dat venster
verspreid te zien is over die twee schermen. Je kunt daar wel rondwerken
door in plaats van je venster te maximizen 'dock left' en 'dock right' te
gebruiken; de sneltoetsen daarvoor zijn superkey + pijl links en
superkey + pijl rechts.

Volgens mij moet het wel mogelijk zijn om je systeem te laten denken dat
er echt drie schermen zijn, en dat je die dan alle drie apart kunt configureren
(resolutie, oriëntatie, positie). Er bestaat een
[dummy video driver voor xorg](https://packages.ubuntu.com/bionic/xserver-xorg-video-dummy)
maar ik heb daar nog niet erg hard mee kunnen experimenteren. Bovendien ben ik
wat terughoudend om met de Xorg-configuratie te prutsen, zeker omdat die
tegenwoordig niet meer expliciet nodig is.

Een tweede probleem dat ik ondervond,
is de vertraging waarmee het beeld op de tablet getoond
wordt. Ik ben er nog niet uit of dat komt door de latency van het
netwerk, of dat de tablet gewoon traag rendert. Voor een terminalvenster is
de vertraging aanvaardbaar, maar als je er iets fancy
op wil runnen, zoals Hipchat (Hipchat? Fancy? Ik snap niet waarom een
IM-toepassing zo veel tijd nodig heeft om te renderen, maar dat is een
ander verhaal), dan is het niet zo handig.

Nog een derde issue: de VNC server lijkt de 'key repeat functionality'
soms uit te zetten. Ik wil zeggen: als je bijvoorbeeld op backspace duwt,
en je houdt die ingedrukt, dan wordt er toch maar 1 karakter verwijderd,
in plaats van te blijven verwijderen tot je de toets weer los laat.
Mocht je dit ondervinden, typ een keer `xset r on`. Doet dat nog steeds niets,
geef dat commando nog een aantal keer in, en dan zal het wel werken.
(De log-output van je VNC-server vertelt hoe vaak je het moet doen.)

## Conclusie

Een tablet als derde scherm: het werkt met xrandr en x11vnc, maar met een paar
beperkingen. Probeer
het eens uit, en mocht je nog tips hebben om deze oplossing te verfijnen,
geef ze zeker door.
