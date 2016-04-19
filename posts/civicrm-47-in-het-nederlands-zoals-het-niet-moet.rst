.. title: CiviCRM 4.7 in het Nederlands, zoals het niet moet
.. slug: civicrm-47-in-het-nederlands-zoals-het-niet-moet
.. date: 2016-04-19 11:26:46 UTC+02:00
.. tags: civicrm,development
.. link:
.. description: Een lelijke hack rond de vertalingsissues van CiviCRM 4.7
.. type: text

Wij gebruiken `CiviCRM <https://civicrm.org>`_ op het werk, een cool en
open CRM-systeem om onze contacten, evenementen en publicaties te beheren.
Toen we er in oktober mee startten, gebruikten we CiviCRM 4.6. Maar sinds
januari is
`CiviCRM 4.7 <https://civicrm.org/blog/yashodha/civicrm-470-is-here>`_ uit,
met allerlei nieuwe coole features die we willen gebruiken.

Maar. Er is een probleem.

.. TEASER_END

Onze CiviCRM-instantie is vertaald in het
Nederlands (of toch iets dat daarop lijkt ;)), want onze gebruikers spreken
Nederlands. Maar CiviCRM 4.7 is niet volledig vertaald in het Nederlands,
`en het is niet vanzelfsprekend om dat op te lossen <https://forum.civicrm.org/index.php?topic=37086>`_.

In theorie komen alle vertaalbare tekstfragmenten uit CiviCRM automatisch
in het `transifex-project van CiviCRM <https://www.transifex.com/civicrm/civicrm/>`_
terecht. Via een webinterface kan CiviCRM dan vertaald worden, en die vertalingen
kun je dan downloaden voor je CiviCRM-instantie. Maar de koppeling tussen
de broncode van CiviCRM en transifex
`is stuk <https://issues.civicrm.org/jira/browse/CRM-17737>`_.

Ikzelf heb momenteel geen tijd om uit te vissen wat er mis is. Bovendien
moet ik toegeven dat ik daar ook geen zin in heb ;-). Maar ik denk niet dat
ik ermee weg kom om onze CiviCRM te upgraden naar 4.7, zonder dat de
vertaling min of meer in orde is. Dus maakte ik
een quick en dirty workaround, in de vorm van een CiviCRM-extensie:
`dutch47 <https://civicrm.org/extensions/dutch47>`_. Deze extensie voert de
vertalingen uit die ontbreken in een standaardinstallatie van CiviCRM in het
Nederlands.

Ik hoop dat het vertalingsprobleem van CiviCRM snel opgelost wordt, want
ik vermoed dat wij niet de enigen zijn die CiviCRM gebruiken in een andere
taal dan het Engels. Maar zo lang dat niet het geval is, kun je
`dutch47 <https://civicrm.org/extensions/dutch47>`_ gebruiken om de
Nederlandse vertaling van CiviCRM te vervolledigen.

De kans is evenwel groot dat ik een aantal tekstfragmenten over het hoofd zag.
Als je er zo vindt, dan ben je uitgenodigd om de extensie te verbeteren.
De code staat op
`github <https://github.com/Chirojeugd-Vlaanderen/dutch47>`_,
en pull requests zijn welkom.
