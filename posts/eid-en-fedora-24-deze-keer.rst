.. title: eID en Fedora. 24 deze keer.
.. slug: eid-en-fedora-24-deze-keer
.. date: 2016-11-14 22:29:37 UTC+01:00
.. tags: eid,fedora
.. category: 
.. link: 
.. description: 
.. type: text

Bijna iedere keer als je je eID wilt gebruiken onder Linux, moet dat anders.
Die indruk heb ik tenminste. Vandaag lukte het na veel proberen op deze manier:

#. Ik heb de middleware uiteindelijk handmatig gebuild, van de tar.gz-source die
   ik downloade van http://eid.belgium.be/en/using_your_eid/installing_the_eid_software/linux. 
   Achteraf bekeken was dat misschien niet nodig. Hoe dan ook, ik moest deze pakketten
   installeren om de middleware te kunnen builden:

.. code-block::

    sudo dnf install pcsc-lite-libs pcsc-lite-devel gtk3-devel

#. Blijkbaar moet ook de pcscd-service gestart zijn, dat is iets voor de usb-kaartlezer.

.. code-block::

    sudo service pcscd start

Zo werkte de eID. Dit keer had ik ook Java nodig. Waarschijnlijk om de security
van de eID terug wat ongedaan te maken. Ik installeerde de rpm van
https://www.java.com/en/download/linux_manual.jsp, maar dan moest ik ook nog
de browser plug-in kunnen gebruiken:

.. code-block::

    ln -s /usr/java/jre1.8.0_111/lib/amd64/libnpjp2.so ~/.mozilla/plugins

Allez, we kunnen weer even verder. Tot volgende week waarschijnlijk, wanneer
Fedora 25 gereleaset wordt :-/

