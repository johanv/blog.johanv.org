.. title: Een minimum aan git
.. slug: een-minimum-aan-git
.. date: 2016-04-02 19:38:22 UTC+02:00
.. tags: git,development
.. link:
.. description: Minimum minimorum git-kennis voor iedereen die wel eens wat moet programmeren.
.. type: text

(Disclaimer: Deze tekst heeft nog wel wat opkuis nodig.)

Van zodra je eens een programmeeropdracht moet doen die iet of wat complex
is, moet je eigenlijk een versiebeheersysteem gebruiken. Zo'n
versiebeheersysteem helpt je bij te houden welke wijzigingen je maakte aan
je broncode. Stel dat je een aantal wijzigingen maakte, en je bedenkt je
dat je het toch beter op een andere manier zou aanpakken, dan kun je
die wijzigingen makkelijk ongedaan maken. Of als je merkt dat iets niet
meer werkt sinds enige tijd, kun je makkelijk zien wat er sinds dien veranderd
is, en dat kan je dan helpen om het probleem te localiseren.

Als je iets serieus moet programmeren, dan gaat een versiebeheersysteem
meer dan eens je hachje redden. Je zou dus denken dat het gebruik daarvan
opgenomen is in elke cursus programmeren. Maar dat bijkt niet het geval.
En daarom deze blog post.

.. TEASER_END

In dit voorbeeld gebruik ik git, omdat ik daar zelf het beste in thuis ben. Git is
erg krachtig, je kunt er vanalles mee, maar in deze post toon ik het
minimum dat je moet kennen om de geschiedenis van je broncode te bewaren.
De uitleg hier is voor Windows, wellicht nog het meest gebruikte OS voor
beginnende programmeurs.

Git installeren
===============

Eerst en vooral moet je git installeren. Ik ben erg tevreden over Git
Extensions. De installer download je hier:
https://github.com/gitextensions/gitextensions/releases/latest.

Ik stel voor dat je de complete setup downloadt, op dit moment is dat
GitExtensions-2.48.05-SetupComplete.msi.

Een paar aandachtspunten voor de installatie:

* Bij de stap 'required software', vink je msysgit en kdiff3 aan.
* Als je de vraag krijgt 'select ssh client', dan zou ik 'openssh' kiezen.
* Voor de rest kies je maar de defaults.

Als je Git Extensions voor de eerste keer start, dan zal het protesteren
omdat je geen naam of e-mailadres hebt geconfigureerd. Je vult dat best
in, die gegevens worden enkel gebruikt om te bewaren wie welke code
schreef.

Mijn demo-project
=================

Bij wijze van illustratie maakte ik een minimaal softwareprojectje, met
1 bestand broncode:

.. image:: /galleries/Git_Demo/git01.png

Dit is de inhoud van dat bestand (het is maar een demo, he):

.. code-block:: python

  print('Hello world');


Je repository initialiseren
===========================

Nu gaan we een 'repository' maken voor dat project: een soort van database
die de versies van jouw source code zal bijhouden.

Om dit te doen, open je Git Extensions, je klikt 'Create new repository'.
Klik dan op 'Browse' om de folder waarin je source staat te selecteren.
Als repository type kies je 'Personal repository', en vervlogens klik
je op 'Create'.

.. image:: /galleries/Git_Demo/git02.png

Je krijgt bevestiging op je scherm dat er een lege repository is gemaakt.

.. image:: /galleries/Git_Demo/git03.png

In het project is nu een verborgen folder aangemaakt: .git. Daarin zal de
geschiedenis van je code bewaard worden, verder moet je je er niets van
aantrekken. Je moet hem daar vooral laten staan.

Initiële commit
===============

Je repository is nu nog leeg. Je zult zelf moeten aangeven welke versies van
je project bewaard moeten worden. We spreken over 'commits'. De eerste
commit die we zullen maken, zal de huidige source code bevatten. Klik
hiervoor op de rode 'Commit' knop in Git Extensions:

.. image:: /galleries/Git_Demo/git04.png

Je krijgt nu een overzicht. Linksboven zie je de bestanden uit je project
die nieuw of gewijzigd zijn. Op dit moment is nog alles nieuw. Linksonder
komen de wijzigingen die je wilt bewaren in je commit.

.. image:: /galleries/Git_Demo/git05.png

Door op de paarse pijl-omlaag te klikken (Stage), kun je bestanden van
boven naar beneden verplaatsen. De dubbele pijl naar beneden 'staget' alle
nieuwe en gewijzigde bestanden. (Als je een bestaand project importeert,
en daarin zitten gecompileerde bestanden zoals bijv. pyc-bestanden voor
python, let er dan op dat je die niet staget. Daarover later meer.)

.. image:: /galleries/Git_Demo/git06.png

Ons project bestaat maar uit 1 bestand. Dus we stagen dat bestand. In het
venster rechtsonder tik je wat uitleg over je commit. En ten slotte klik
je op de knop Commit.

.. image:: /galleries/Git_Demo/git07.png

Je krijgt wat feedback over je commit, die mag je wegklikken.

.. image:: /galleries/Git_Demo/git08.png

En daarna zie je dat je git repository één versie van je source code
bevat.

.. image:: /galleries/Git_Demo/git09.png

Wijzigingen en volgende commits
===============================

Nu gaan we aan onze code werken. Ik maak mijn programma wat
gesofisticeerder.

.. code-block:: python

  import datetime

  d = datetime.datetime.now()
  if d.hour >= 17:
      print('Good evening world')
  else:
      print('Hello world')

Als je nu terug naar Git Extensions gaat kijken, en je klikt 'commit',
dan krijg je mooi te zien wat er veranderd is.

.. image:: /galleries/Git_Demo/git10.png

Ik 'stage' het gewijzigde bestand, schrijf een korte uitleg over wat ik
veranderde, en klik opnieuw commit.

Ik doe nog een kleine aanpassing (leestekens in mijn boodschap):

.. code-block:: python

  import datetime

  d = datetime.datetime.now()
  if d.hour >= 17:
      print('Good evening world.')
  else:
      print('Hello world.')

En commit opnieuw. Merk weer op hoe de wijzigingen worden gevisualiseerd.

.. image:: /galleries/Git_Demo/git11.png

.gitignore
==========

In een volgende wijziging splitste ik mijn project op in 2 bestanden:
het bestand 'hello.py':

.. code-block:: python

  import datetime
  import hellohelper

  d = datetime.datetime.now()
  hellohelper.say_hello(d)

En een bestand 'hellohelper.py':

.. code-block:: python

  def say_hello(d):
      if d.hour >= 17:
          print('Good evening world.')
      else:
          print('Hello world.')

Toen ik dit wilde committen, kreeg ik het volgende:

.. image:: /galleries/Git_Demo/git12.png

Git heeft gemerkt dat er een binair pyc-bestand bijgekomen is. Dat is
een gecompileerde python module, en het is niet de bedoeling dat we die
bij opnemen in onze repository: die wordt namelijk gemaakt op basis van
de broncode in onze .py-bestanden. Wat we zouden kunnen doen, is enkel de
py-files stagen en committen.

Maar we kunnen git ook duidelijk maken dat pyc-bestanden genegeerd mogen
worden voor de repository. Dat kun je doen door rechts te klikken op dat
pyc-bestand, en dan 'Add file to .gitignore' te klikken.

.. image:: /galleries/Git_Demo/git13.png

Bij 'enter a file pattern to ignore', stond er in mijn geval
``__pycache__/hellohelper.cpython-34.pyc``. Dat veranderde ik in
``*.pyc``,
waarmee git weet dat alle bestanden met pyc-extensie genegeerd mogen
worden.

.. image:: /galleries/Git_Demo/git14.png

Klik op ignore. Je zult zien dat er nu een nieuw bestand '.gitignore' is
gemaakt in je repository. Daarin staat ``*.pyc``, en op die manier weet
git dat alle pyc-bestanden genegeerd mogen worden. Het
.gitignore-bestand bewaren we ook in onze repository, samen met de
andere wijzigingen.

.. image:: /galleries/Git_Demo/git15.png

Door je geschiedenis bladeren
=============================
In Git Extensions zie je nu je vier commits. Elke commit komt overeen
met een versie in je broncode. Als je er een
aanklikt, en je klikt in het onderste paneel op 'Diff', dan kun je zien
welke wijzigingen er in iedere commit zijn gebeurd. De 'commit messages'
helpen je te onthouden wat je bedoeling was met iedere wijziging.

.. image:: /galleries/Git_Demo/git16.png

Als je 2 commits aanklikt (met ctrl-klik), dan krijg je de verschillen
tussen die twee versies.

.. image:: /galleries/Git_Demo/git17.png

En nu?
======
Nu ken je de basis. Iedere keer je iets nieuws hebt geïmplementeerd, of
iedere keer als je een fout opgelost hebt uit je code, maak je een
nieuwe commit, en omschrijf je wat je hebt gedaan. Je kunt ook op
'commit' klikken om te bekijken wat je sinds je laatste commit hebt
veranderd. Als je dan nog niet meteen een nieuwe commit wilt maken, klik
je het venster gewoon opnieuw weg.

.. image:: /galleries/Git_Demo/git18.png

Maak er een gewoonte van om regelmatig te committen. Zo behoud je het
overzicht over wat je allemaal veranderde of bijprogrammeerde.

Op termijn kun je git gebruiken om meerdere versies van je code te
beheren. Of versies met en zonder een nieuwe experimentele feature. Git
kan je ook helpen om veilig met meer personen aan een project te werken.
Als je dit nodig hebt, dan vind je wellicht een git-kenner die het je
uitlegt. Maar dat zien we dan wel weer. Voorlopig weet je hoe je commits
kunt bewaren, en nogmaals: doe dat bij iedere nieuwigheid die je (al dan
niet gedeeltelijk) maakt, en iedere bug die je fixt.

Succes!
