## Intro

histoire du mercredi matin et "il nous faut des tests automatiques"

me présenter

Mon vrai but est d'emmener la communité C++ à faire plus de tests unitaires

finalement, c'est tests automatiques dans code existant.  appuyer avec "dans toute ma carrière, je n'ai fait qu'un seul projet avec TDD"

"c'est de cela que je vais vous parler de soir.  Comment inoculer nos programmes contres les bogues avec des tests automatiques"

pas facile, commettre des crimes

## Plan

Pourquoi?
Commencer
????

## Pourquoi des tests?

Mon patron demande "qu'est-ce que ça donne des tests"?

évite les tâches répétitives, donc on gagne en productivité

Votre connaissance de tous les cas à supporter est maintenant documentée

on peut améliorer le code sans peur: bcp moins de risques

on peut tester des changements plus distants: compilateur, plateforme, librairie, etc

pour pouvoir avoir un meilleur design plus flexible

Éventuellement, quand vous ferez TDD, vos solutions seront différentes


## Commencer

choisir une librairie de test
- Microsoft Unit Test Framework, CppUnitTest
- GoogleTest
- Boost Unittest
- Catch2

Catch2 parce que:
- expressions de test sont naturelles
- support surprenant mais utile des "fixtures"
- comparaisons des nombres flottants
- matchers
- match exceptions, en détail

intégrer au build system

intégrer l'exécution à votre cycle de développpement
- dans la Pull Request de GitHub
- compilation durant la nuit
- auto compilation sur la consignation

décider où placer le code de test
