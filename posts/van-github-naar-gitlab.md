<!--
.. title: Van github naar gitlab
.. slug: van-github-naar-gitlab
.. date: 2019-05-11 10:16:15 UTC+02:00
.. tags: johanv.org, git, ci, nikola
.. category: 
.. link: 
.. description: 
.. type: text
-->

Mijn website is een statische website. Dat wil zeggen dat de hele site automatisch wordt gegenereerd op basis van een aantal tekstbestanden. En die tekstbestanden beheer ik met git.

Gisteren verhuisde ik de git-repository met die teksten van [github](https://github.com/johanv/blog.johanv.org) naar [gitlab](https://gitlab.com/johanv/blog). Niet omdat ik problemen heb met het [bedrijf achter github](https://blogs.microsoft.com/blog/2018/06/04/microsoft-github-empowering-developers/) (want Microsoft is tegenwoordig érg goed bezig), maar wel omdat ik [gitlab-ci](https://about.gitlab.com/product/continuous-integration/) wou gebruiken om mijn site te bouwen. Concreet komt het erop neer dat, telkens ik een nieuwe versie van de source push, gitlab voor mij mijn site [automatisch bouwt en deployt](https://gitlab.com/johanv/blog/pipelines).

Misschien heeft github ook wel zo'n service. Maar die van gitlab heb ik nu al vaak gebruikt, en ik ben er erg tevreden over. Dus vanaf nu wordt mijn blog gepowerd door [nikola](https://getnikola.com) en gitlab. Waarvoor dank.