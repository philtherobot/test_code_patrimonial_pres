## Intro

me présenter
- nom
- C++ depuis 29 années
- informatique mobile, télécom, serveurs de courriels, modélisation des transports, traitement de donnés radar-laser.

présenter la présentation
- parlera de tests automatiques, ceux qui font parti du projet, parti de l'étape de développement, pas ceux qu'on appelle "GUI test automation"

motivation personelle, content et excité
- changé ma façon de programmer pour le mieux
- je désire partager cette expérience

## Plan

mon plan pour vous...
- Unitaire versus integration, les diffs
- Pourquoi?
- les ingrédients de base pour commencer
- Exemples

## Unitaire versus intégration

définition, se comprendre
- AQ, manuel
- intégration, demande infrastrucure externe ou lourd traitement
- unitaire, une fonction ou classe à la fois, rapides

unitaire
- si un test échoue, le code en cause est le code sous test
- l'objectif idéal est l'intégration a votre cycle de développement à chaque minute, chaque compilation,
- sinon à la consignation

intégration
- teste les interactions entre fonctions et classes
- difficiles
- peut être exécutés avant la livraison

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
- infaillible et complet
- si vous changez le code, la docum devra suivre

librairie de tierce partie
- libre de changer, tester régression
- même chose compilateur et plateforme

pour pouvoir avoir un meilleur design plus flexible
- briser les dépendances, sinom intestable
- inversion des dépendances
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
- support surprenant mais utile des "fixtures"
	- fixture: souvent, plusieurs tests partagent la même préparation ou initialisation.  Un bon framework de test va structurer et faciliter la réutilisation de l'initialisation.
- comparaisons des nombres flottants
	- par epsilon
	- par pourcentage
- matchers - prédicat
	- Starts with, contains etc
- match exceptions, en détail
- exclure/include les tests à exécuter

intégrer au build system
- les tests, c'est généralement un programme exécutable: targets
- le plus simple: ajouter un répertoire "test" à la racine
- si vous avez des modules, un rép "test" pour chaque

intégrer l'exécution à votre cycle de développement
- dans la Pull Request de GitHub
- compilation durant la nuit
- exécution à
- la consignation
- si manuel: ajouter au processus de livraison


## Exemples

trouvé deux beaux exemples dans vcpkg
- vcpkg parle git
- git status --porcelain
- parse_git_status_output, sortie, vector of GitStatusLine

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

--> prêt à faire la voix

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

il faut un simulacre
- soit on ajoute un nouveau paramètre
- soit une factory 
	- en prod, le vrai objet
	- en test, un objet injecté par le test
	
--> cont

autres cas
- fonction qui a besoin ou créera de petits fichiers, on y va directement si on veut
- dériver et override les fonctions problématiques qui empêche les tests

les interfaces du code sous test peut être
- une classe avec des méthodes virtuelles
- le code sous test peut être générique (template)

## Conclusion

J'espère que cette présentation vous aidera à implanter des tests automatiques dans votre projet

Merci, bonne soirée


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
