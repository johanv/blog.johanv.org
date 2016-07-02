.. title: Nog een dikke week voor de belastingsaangifte...
.. slug: nog-een-dikke-week-voor-de-belastingsaangifte
.. date: 2016-07-02 21:43:49 UTC+02:00
.. tags: overheid,debian
.. link: 
.. description: Aanloggen met de eID. Hoe moest het ook alweer...
.. type: text

Dus tijd voor mijn jaarlijkse blog post over Tax-On-Web met Linux.

Dit jaar met `Bunsenlabs <https://www.bunsenlabs.org/>`_, maar dat is
eigenlijk een getunede `Debian <https://www.debian.org>`_ 8 (Jessie).

* Download en installeer het Debian package van
  http://eid.belgium.be/nl/je_eid_gebruiken/de_eid-middleware_installeren/linux.
* Dat enablet waarschijnlijk een paar extra repository's. Dan moet je nog
  twee packages installeren:

.. code-block::

    sudo apt-get update
    sudo apt-get install eid-mw eid-viewer

* Dan nog in about:config ``security.tls.unrestricted_rc4_fallback`` op 
  ``true`` zetten. Waarschijnlijk is dat een security isssue.

Dat was het. Browser herstarten, en het werkt. Probeer eventueel de
`test <https://test.eid.belgium.be/>`_ als je niet zeker bent.

Ik heb niet expliciet een browser add-on moeten installeren, maar er is er
wel een geïnstalleerd in mijn Firefoxinstantie: 
Belgium eID 1.0.18.1-signed.1-signed. 
Ik ben niet zeker of die mee kwam met een van de
packages van daarnet. Misschien zat die gewoon nog ergens in
mijn Firefox Sync.

Happy tax-on-webbing!
