.. title: Belgische fiscale attesten met een CDNTaxReceipts-hack
.. slug: belgische-fiscale-attesten-met-een-cdntaxreceipts-hack
.. date: 2016-02-25 17:33:03 UTC+01:00
.. tags: civicrm,hacks
.. link:
.. description: Ik vond een extensie voor fiscale attesten. Voor Canada...
.. type: text

Sinds kort hebben wij een coole `CiviCRM <https://civicrm.org>`_-instantie
op het werk. En één van de dingen dat we daarmee willen doen, is
fiscale attesten afdrukken. Ik dacht dat we waarschijnlijk niet de enigen waren
die zoiets wilden doen, dus ik heb wat gezocht op het Internet. Misschien
zocht ik niet goed genog, want hoewel België wereldwijd
`op de vijfde plaats <https://civicrm.org/blogs/cividesk/how-many-organizations-use-civicrm-where-how>`_
staat qua CiviCRM-implementaties, heeft er nog niemand CiviCRM gebruikt
voor Belgische fiscale attesten. Of er is niemand die eraan gedacht heeft om te
documenteren hoe het kan.

Maar nu dus wel.

.. TEASER_END

Ik zal vertellen hoe het bij ons werkt.

Maar eerst wat disclaimers:

1. **HACK ALERT!** Dit is niet het mooiste stuk code uit onze Civi. Maar
   omdat het allicht bruikbaar is voor anderen, post ik het toch.

2. Ik weet hier eigenlijk niet veel van, dus het kan zijn dat ik dingen gewoon
   fout doe. Laat me gerust weten waar ik de bal overal mis sla.

CDNTaxReceipts
==============
Wat het zoekwerk op internet wel opleverde, was
`CDNTaxReceipts <https://civicrm.org/extensions/cdn-tax-receipts>`_ extensie,
een extensie voor Fiscale attesten in Canada. Ik heb die geïnstalleerd, en
daar wat aan geprutst, tot het werkte voor ons.

CDNTaxReceipts kent twee soorten fiscale attesten: 'individuele' en
'jaarlijkse'. Ik heb geen idee wat die individuele attesten zijn, wij
gebruiken alleen de jaarlijkse.

Patches
=======
Op dit moment, is de recentste versie van CDNTaxReceipts versie 1.3.1, en daar
zijn een aantal issues mee. Alnaargelang je daarmee kunt leven of niet, zul
je patches moeten toepassen. (Als je niet weet dat moet,
zie :doc:`How to apply a patch <how-to-apply-a-patch>`.)

Als je extensies gebruikt in je CiviCRM-instantie, en die extensies definiëren
custom permissies, dan durft CDNTaxReceipts die custom permissies wel eens te
verwijderen. Niet zo fijn. Dit is op dit moment gefixt in de master branch
op github, maar als je versie 1.3.1 van de extensie gebruikt (of vroeger),
dan `moet je patchen <https://patch-diff.githubusercontent.com/raw/jake-mw/CDNTaxReceipts/pull/37.diff>`_.

Versie 1.3.1 van CDNTaxReceipts werkt niet met CiviCRM 4.7. Dus als jij wel
CiviCRM 4.7 gebruikt, apply dan
`deze patch <https://patch-diff.githubusercontent.com/raw/jake-mw/CDNTaxReceipts/pull/44.diff>`_.

Tenslotte verstuurt CDNTaxReceipts ieder gemaakt fiscaal attest by default
naar een e-mailadres dat je moet configureren. De developers van
CDNTaxReceipts vinden dat belangrijk, maar ik vond dat vooral vervelend. Dus
als je dat archief-e-mailadres optioneel wilt maken,
`ook daar is een patch voor <https://patch-diff.githubusercontent.com/raw/jake-mw/CDNTaxReceipts/pull/39.diff>`_

Hooks
=====
CDNTaxReceipts voorziet hooks, die je kunt gebruiken om het verder te
configureren: ``hook_cdntaxreceipts_eligible``, waarmee je kunt bepalen
of een bijdrage (contribution) in aanmerking komt voor een fiscaal attest.
En ``hook_cdntaxreceipts_alter_receipt``, waarmee je aan het attest kunt
foefelen. We gaan die allebei gebruiken.

`Hooks kun je implementeren <https://wiki.civicrm.org/confluence/display/CRMDOC/Hook+Reference#HookReference-Implementinghooks>`_
in een
`eigen CiviCRM-extensie <https://wiki.civicrm.org/confluence/display/CRMDOC/Create+a+Module+Extension>`_,
of, als je onderliggend Drupal gebruikt, in een
`https://www.drupal.org/node/1074360 <eigen Drupal module>`_.

Wij hebben de hooks geïmplementeerd in een CiviCRM-extensie die
``chirocontribution`` heet, zoals je zult zien aan de naam van de
hook-implementaties.

hook_cdntaxreceipts_eligible
----------------------------
Je krijgt in België (op dit moment toch) een fiscaal attest als je op
jaarbasis 40 EUR of meer hebt geschonken. Het is een beetje vervelend dat
``hook_cdntaxreceipts_eligible`` per contributie wordt aangeroepen, want
wat ik uiteindelijk doe, is voor iedere contributie apart nakijken of
diegenen die de gift doet, over heel het jaar voldoende heeft geschonken
om een attest te krijgen. Dit is erg inefficiënt, maar voor ons aantal
giften lijkt het te werken. Hier is de code

.. code-block:: php

    /*
     * Implements hook_cdntaxreceipts_eligible.
     *
     * Checks whether the contact can get a tax receipt for the year of the contribution.
     */
    function chirocontribution_cdntaxreceipts_eligible($contribution) {
      if ($contribution->financial_type_id != 1) {
        return array(FALSE);
      }
      // TODO: More caching.
      $year = date('Y', strtotime($contribution->receive_date));
      $sql = "SELECT SUM(total_amount) FROM civicrm_contribution WHERE contact_id = %1 AND YEAR(receive_date) = %2;";
      $params = array(
        1 => array($contribution->contact_id, 'Integer'),
        2 => array($year, 'Integer'),
      );
      $total = CRM_Core_DAO::singleValueQuery($sql, $params);
      return array($total >= CRM_Core_BAO_Setting::getItem('chirocontribution', 'chirocontribution_amount_tax_receipt'));
    }

Dus in de naam van de hook-implementatie verander je ``chirocontribution`` door
de naam van je eigen extensie en module. Het opmerken waard is deze regel:

.. code-block:: php

    return array($total >= CRM_Core_BAO_Setting::getItem('chirocontribution', 'chirocontribution_amount_tax_receipt'));

Ik heb dus het bedrag van 40 EUR niet letterlijk in de code gezet;
ik gebruik een CiviCRM setting ``chirocontribution_amount_tax_receipt`` uit
de groep ``chirocontribution``. Het idee is om ooit een formulier te maken
via hetwelke we dat bedrag kunnen configureren, maar dat is nog een van de vele
open issues van ons project ;-)

Je kunt die setting bijvoorbeeld als volgt zetten in de
`installer of upgrader <https://wiki.civicrm.org/confluence/display/CRMDOC/Create+a+Module+Extension#CreateaModuleExtension-Addadatabaseupgrader/Installer/uninstaller>`_
van je extensie:

.. code-block:: php

    CRM_Core_BAO_Setting::setItem('40', 'chirocontribution', 'chirocontribution_amount_tax_receipt');

hook_cdntaxreceipts_alter_receipt
---------------------------------
CDNTaxReceipts geeft de fiscale attesten het Contribution-ID van een van de
contributies die bij het attest horen. Nu ken ik niet veel van attestnummers,
maar ons oude systeem genereerde attestnummers in de vorm van jaartal/volgnummer.
Bijvoorbeeld 2015/0004. Ik ben ook eens in de kast gaan kijken naar de
fiscale attesten die ik zelf krijg, en die zijn gelijkaardig genummerd. Dus
ik vermoed dat dat zo dan wel zal moeten in België.

Door ``hook_cdntaxreceipts_alter_receipt`` te implementeren, verander ik
het nummer van het attest alvorens het gemaakt en bewaard wordt. Dit gebeurt
alweer op een erg inefficiënte manier, die bovendien niet thread safe is.
Maar ook dit werkt voor ons; misschien werkt het ook wel voor jullie.
(Ruimte voor verbetering is er altijd.) De code:

.. code-block:: php

    /**
     * Implements hook_cdntaxreceipts_alter_recepit.
     *
     * Calculates receipt number for receipt.
     *
     * @param type $receipt
     */
    function chirocontribution_cdntaxreceipts_alter_receipt(&$receipt) {
      $year = _chirocontribution_receipt_year($receipt);
      if ($year == NULL) {
        throw new Exception("All contributions should have receive_date in the same year.");
      }

      // Only generate a receipt number if it does not exist yet.
      // (see #4842).

      if (!$receipt['is_duplicate']) {
        // FIXME: this is slow, and not thread safe.
        // TODO: make number of digits after slash configurable.
        $sql = "SELECT MAX(CAST(SUBSTRING(receipt_no, 6) AS UNSIGNED)) FROM cdntaxreceipts_log WHERE receipt_no LIKE %1";
        $params = array(
          1 => array( $year . '/%', 'String' ),
        );
        $last = CRM_Core_DAO::singleValueQuery($sql, $params);
        $next_no = $year . '/' . str_pad($last + 1, 4, 0, STR_PAD_LEFT);
        $receipt['receipt_no'] = $next_no;
      }
    }

    /***
     * If all contributions of the receipt are in the same year, return that year.
     *
     * Otherwise return NULL.
     */
    function _chirocontribution_receipt_year($receipt) {
      $result = NULL;

      foreach ($receipt['contributions'] as $contribution) {
        $contribution_year = date('Y', strtotime($contribution['receive_date']));
        if ($result == NULL) {
          $result = $contribution_year;
        }
        else if ($result != $contribution_year) {
          return NULL;
        }
      }
      return $result;
    }

Achteraf zal blijken dat CDNTaxReceipts de fiscale attesten zal bewaren met een
bestandsnaam die het attestnummer bevat. Dat is wat vervelend met die slash,
maar daar werken we straks nog rond. Wat je ook kunt doen, is een punt (.)
gebruiken in plaats van een slash, in dat geval vervang je de slashes in
de ``$params`` van de select query en in ``$next_no`` door een punt.

Hacks
=====
We zijn er nog niet. De fiscale attesten van CDNTaxReceipt worden met 3 op
een blad geprint (telkens 1 origineel en 2 dubbeltjes, dus 3 keer hetzelfde
attest op één blad), en er staat niet op wat we willen.

Om het voor ons goed te krijgen, heb ik cdntaxreceipts.functions.inc aangepast
`op deze manier <https://github.com/johanv/CDNTaxReceipts/commit/d96d1f655ac388f25279f331934432d853a0d20a.diff>`_.

Wat deze patch doet:

* per contact 1 attest op 1 blad zetten
* alles wat herschikken
* adressen op z'n Belgisch, dus geen provincies (states) na de postcode
* de forward slash vervangen door een underscore in bestandsnamen
* datums op z'n Belgisch
* euro's i.p.v. dollars, en geen komma's om duizendtallen te scheiden

Nadat je ze geapplied hebt, wil je de attesten waarschijnlijk ook aanpassen.
Bij ons staat elke gift er apart op, maar dat heb ik nog nergens anders zo
gezien. Bovendien staat er daar ook overal 'PCH' hardgecodeerd achter. Geen
idee waarom. Vermoedelijk is dat irrelevant of fout, maar dat heeft dan nog
niemand gerapporteerd ;-)

Wat je dus moet doen, is het bestand
`cdntaxreceipt.functions.inc <https://github.com/johanv/CDNTaxReceipts/blob/chiro/cdntaxreceipts.functions.inc#L266>`_
nog wat verder customizen tussen de lijnen 266 en 466. Als je de aparte
bedragen eruit wilt, dan verwijder je de foreach-structuur op lijnen 347-354.
Je ziet maar wat je ermee doet.

Hoe werkt het nu?
=================

Configuratie
------------
De extensie is behoorlijk configureerbaar, en dat doe je via het CiviCRM
menu: 'beheer', 'civicontribute', 'cdntaxreceipts'.

Attesten maken
--------------
Dit is min of meer onze gebruikershandleiding:

1. Klik op 'Contacten', 'Geavanceerd zoeken'.
2. Klik het flapje 'Bijdragen' open.
3. In de sectie 'Bijdragen', kies je bij 'Ontvangstdatum' 'Vorig jaar', en je
   zorgt ervoor dat je enkel de contributies filtert die relevant zijn voor
   de fiscale attesten. (Bij ons zijn dat die contributies met als
   'Financieel type' kies 'Donatie', maar ik weet niet of dat iets universeels
   is).
4. Klik 'Zoeken'. Je krijgt een lijst met alle personen die vorig jaar een gift deden.
5. Klik 'Alle X records' aan bij 'Selecteer records'.
6. Klik onder 'Acties' op 'Issue Annual Tax Receipts'.
7. Wacht even.
8. Kijk na of het juiste werkjaar is aangevinkt.
9. Klik 'Issue tax receipts'.
10. Wacht lang.
11. Download de PDF.

(Je kunt ook een preview maken van de tax receipts, maar ten gevolge van
hoe we de attestnummers berekenen, hebben in die preview alle attesten
hetzelfde nummer.)

Attesten herafdrukken
---------------------

1. Zoek de persoon op waarvoor het fiscaal attest afgedrukt moet worden.
2. Klik op het flapje 'Bijdragen'.
3. Klik op 'weergeven' achter een gift uit het jaar waarvoor het attest herafgedrukt moet worden.
4. Klik op de knop 'Tax Receipt'.
5. Klik op de knop 'Re-Issue Tax Receipt'.
6. Bewaar de PDF.

Rapportering
------------
De gegevens van de fiscale attesten moeten doorgegeven worden aan de overheid,
en dat gebeurt bij ons via een csv-export. Op die export staat telkens een
attestnummer, naam, adres en totaalbedrag. De overheid zou graag ook het
rijksregisternummer hebben op dat overzicht, maar de privacywetgeving zegt
ons dan weer dat we geen rijksregisternummers mogen opvragen of bewaren. Wie
dat begrijpt, mag me dat eens komen uitleggen.

In CDNTaxReceipt zit een template voor CiviReport, maar daar staat geen
adres op. Het is waarschijnlijk niet moeilijk om die template aan te passen,
zodat er wel een adres op staat, maar toen ik aan dat rapport begon te werken,
was ik helemaal vergeten dat die template bestond. Ik ben nogal eens
verstrooid of vergeetachtig, helaas. Ik heb dus een custom search gemaakt.

Custom searches maken is een vak apart, en de
`documentatie <https://wiki.civicrm.org/confluence/display/CRMDOC/Create+a+Custom-Search+Extension>`_
is in mijn ogen erg onduidelijk. Misschien schrijf ik daar bij gelegenheid ook
nog eens een blog post over.

Ik zal alleszins de query meegeven die ik gebruik voor de custom search;
desnoods kun je hem runnen op je database, en met dat resultaat iets verder
doen. Voor 2016 ziet de query er zo uit:

.. codeblock sql

    SELECT tr.receipt_no, c.display_name, a.street_address, a.postal_code, a.city, tr.receipt_amount
    FROM cdntaxreceipts_log tr
    JOIN civicrm_contact c ON tr.contact_id = c.id
    LEFT OUTER JOIN civicrm_address a ON c.id = a.contact_id and a.is_primary = 1
    WHERE tr.is_duplicate = 0 AND issue_type = 'annual'
    AND tr.receipt_no LIKE '2016/%'
    ORDER BY receipt_no

Als je een custom search implementeert, dan zet je uiteraard niet letterlijk
`'2016/%'` in je query, je gebruikt een query-parameter (alweer zo'n
ongedocumenteerd iets in CiviCRM-land) die je dan invult
op basis van het jaar dat de gebruiker kiest in je search form. Een aparte
blog post dringt zich meer en meer op ;-)

De slash in de query,
is dezelfde slash die we gebruiken in het attestnummer. Als je dus een ander
scheidingsteken gebruikt in je attestnummer, pas dat dan ook aan in de
waarde van je query parameter.

Al bij al
=========
Eigenlijk is het pas nu ik het allemaal eens uitschrijf, dat ik weer eens
zie hoe hacky het allemaal is. Misschien heeft er iemand wel een mooiere
oplossing, of bestaan er wel betere extensies. Als je het weet, documenteer het
dan. Want voorlopig is dit - waarschijnlijk en helaas - de enige documentatie
op het Internet over hoe je Belgische fiscale attesten maakt met CiviCRM.
