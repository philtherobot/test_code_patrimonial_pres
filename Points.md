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

Catch2 parce que:
- support surprenant mais utile des "fixtures"
- comparaisons des nombres flottants
- matchers - prédicat
- match exceptions, en détail

intégrer au build system

intégrer l'exécution à votre cycle de développement
- dans la Pull Request de GitHub
- compilation durant la nuit
- auto compilation sur la consignation

décider où placer le code de test


## Comment

fonction ou classe simple

fonction qui a besoin ou créera de petits fichiers

découpler pour entourer le code sous test d'interfaces

les interfaces du code sous test peut être
- une classe avec des méthodes virtuelles
- le code sous test peut être générique (template)

## Conclusion

J'espère que cette présentation vous aidera à implanter des tests automatiques dans votre projet

Merci, bonne soirée

----
-utilité questionnable-

il faudra peut être modulariser et réduire la taille des systèmes, pour éviter de faire plus ce qui s'appellerait un test d'intégration mais bien faire un test plus unitaire.

