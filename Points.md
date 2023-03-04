
https://www.canva.com/

## Intro

me présenter
- Philippe Payant
- C++ depuis 29 années
- informatique mobile en 1995, 
- télécom modem et radio début 2000
- serveurs de courriels
- modélisation des transports
- traitement de donnés radar-laser

motivation personelle, content et excité
- changé ma façon de programmer pour le mieux
- je désire partager cette expérience
 
présenter la présentation
- parlera de tests automatiques par les programmeurs
- tests unitaires et intégration
- versus "GUI test automation" par l'équipe AQ


## Plan

mon plan pour vous...
- types de tests, unitaire, intégration, infrastructure, les diffs
- Pourquoi?
- les ingrédients de base pour commencer
- Exemples

## Types de tests

définition, se comprendre
- unitaire, 
	- une fonction ou classe à la fois
	- rapides
	- peu ou pas de deps externes
- intégration
	- tout ou grande partie de l'appli
	- lourd traitement?

unitaire
- si un test échoue, le code en cause est le code sous test
- l'objectif idéal est l'intégration a votre cycle de développement à chaque minute, chaque compilation,
- sinon à la consignation

intégration
- teste les interactions entre fonctions et classes
- peut être exécutés avant la livraison

dépendances externes
- processus externe, BD, serveur HTTP
- fragilise l'exécution des tests et exige un montage (setup)
- isolation et simulacre (mocking)


## Pourquoi des tests?

1ère R, évite les tâches répétitives
- un projet sans TA, il faut tester "à la main", comme un usager, utiliser l'application
- avec TA, on teste des parties de l'appli en une fraction de seconde, on gagne en productivité

on peut améliorer le code sans peur
- si un composant est protégé par des tests
- on peut le changer, extraire, réordonner, changer complètement l'algo
- liberté de changer est plus grande, très grande

auto documentation
- en lisant, entrées -> sorties, corner cases
- infaillible et complète
- si vous changez le code, la docum devra suivre

changer deps
- libre de changer, tester régression
- même chose compilateur et plateforme

pour pouvoir avoir un meilleur design plus flexible
- doit isoler pour tester
- souvent accompli par inversion des deps
- code devient utilisable dans de nouveaux contextes

permet de tester des situations difficiles à produire
- qui a déjà changé l'heure
- avec un simulacre d'horloge, OK
- un simulacre de serveur HTTP, même chose


## Commencer

choisir un framework de test
- Microsoft Unit Test Framework, CppUnitTest
- GoogleTest
- Boost Unittest
- Catch2

Catch2 mon préféré
- mais votre choix devrait avoir
- montures (fixtures)
	- fixture: souvent, plusieurs tests partagent la même préparation ou initialisation.  Un bon framework de test va structurer et faciliter la réutilisation de l'initialisation.
- comparaisons des nombres flottants
	- par epsilon relatif
	- par distance absolue
- comparaisons 
	- vector
	- pour type de vocabulaire de l'appli
- matchers - prédicat
	- Starts with, contains etc
- match exceptions, en détail
- exclure/include les tests à exécuter

intégrer au build system
- les tests, c'est généralement un programme exécutable: cibles
- le plus simple: ajouter un répertoire "test" à la racine
- si vous avez des modules, un rép "test" pour chaque

intégrer l'exécution à votre cycle de développement
- difficile sans intégr continue
- dans la Pull Request
- compilation durant la nuit
- exécution à la consignation
- sans intég continue, faites votre choix

## Exemples

trouvé deux beaux exemples dans vcpkg
- vcpkg parle git
- git status --porcelain
- parse_git_status_output analyse, vector of GitStatusLine en sortie

*Exemple vcpkg, parse_git_status_output [src/vcpkg/base/git.cpp](https://github.com/microsoft/vcpkg-tool/blob/b1ea41f0dbe82ee2e8e5437fd93d8ab0b52c2d6a/src/vcpkg/base/git.cpp#L49)*

(montrer src/vcpkg-test/git.parse.cpp)
- les différentes entrées
- test avec entrée vide
- test avec bonne entrée et vérification
- tests de mauvaises entrées

cas le plus simple
- pas d'interaction avec l'extérieur
- simple à faire
- couvre bien
- possible grâce au design: l'exécution de git est séparée de l'analyse

*Exemple vcpkg, git_status*

(montrer git_status)
- construit la ligne de commande
- appelle cmd_execute_and_capture_output
- traite erreur

pour tester
- il nous faudrait 
	- un clone git
	- git installé
	- façon de produire une erreur exit != 0
	- un problème au lancement (pas de git installé?)

(montrer la [structure du test](https://godbolt.org/z/xbjr1nePs))
il faut un simulacre
- soit on ajoute un nouveau paramètre
- teste la ligne de commande capturée

 (montrer les [alternatives](https://godbolt.org/z/fnc161fGP))
- haute perf, template
- soit une factory 
	- en prod, le vrai objet
	- en test, un objet injecté par le test

autres cas, vous avez du legacy

- si facile d'utiliser un bac à sable
	- on y va directement

- refactoriser la fonction afin de séparer l'écriture au fichier
	- fonction interne produit une chaîne, celle que l'on teste
	- fonction externe écrit la chaîne dans le fichier, non-testé

- dériver et override les fonctions problématiques qui empêche les tests

legacy = soyez créatifs

## Conclusion

"Nous avons maintenant parcouru le chemin vers les tests.  En partant des motivations fondamentales, aux outils, à l'intégration jusqu'à l'implantation de vos premiers tests.

Tout ça est évidemment beaucoup de travail, de questions, de défis et de dérangements.  Mais n'oubliez pas les avantages des tests automatiques: moins de travail répétitif, plus grande liberté de changement, l'auto documentation, l'amélioration au design et tous les autres dont on a parlé.

Si vous voulez faire le saut vers les tests automatiques, je vous souhaîte que l'opportunité se présente dans votre environnement de travail: que votre patron soit d'accord et que l'équipe embarque.

En tous les cas, j'espère que cette présentation vous a plu qu'elle pourra vous aider à implanter des tests automatiques dans votre projet.

Merci, bonne soirée!"


---
links
https://www.jamesshore.com/v2/projects/nullables/testing-without-mocks

---
-utilité questionnable-

## Quoi tester

*brisé, pas encore préparé*
il y a du code simple et du code complexe.
il y a du code stable et du code constamment en évolution
nous devrions prioriser complexe et en constante évolution
*link [Prioritizing Technical Debt as If Time & Money Matters](https://www.youtube.com/watch?v=w9YhmMPLQ4U)*

----
-utilité questionnable-

il faudra peut être modulariser et réduire la taille des systèmes, pour éviter de faire plus ce qui s'appellerait un test d'intégration mais bien faire un test plus unitaire.

---
-utilité questionnable-

[exemple 1](https://godbolt.org/z/8o8jETrjG)
