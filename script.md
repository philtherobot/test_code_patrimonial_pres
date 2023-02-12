échéancier

| étape | pts | #sems |
|--|--|--|
| squelette | 1 | 1 |
| texte complet et détaillé | 5 | 5 |
| slides | 3 | 3 |
| apprendre/répéter | 2 | 2 |

## Introduction

One example of a software feature that is complex enough that automatic tests should be used to develop it is a financial transaction processing system.  The feature would require a lot of different validation rules, such as checking that the account has enough funds, that the credit card number is valid, that the transaction is within certain limits, and that the transaction is not a duplicate.

-or-

One example of a software feature with options and exceptions in the Microsoft Office Suite is the spell checker feature in Microsoft Word.

-   Ignoring words in uppercase: This option allows the spell checker to ignore words that are in all uppercase, such as acronyms.
-   Ignoring words with numbers: This option allows the spell checker to ignore words that contain numbers, such as "eBay" or "iPhone".
-   Ignoring internet and file addresses: This option allows the spell checker to ignore web and file addresses, such as "[www.example.com](http://www.example.com)" or "C:\example\file.docx".
-   Adding words to the custom dictionary: Users can add words that the spell checker flags as misspelled but are in fact spelled correctly, such as a specific technical term or a name of a person.
-   Setting the language of the document: Users can set the language of the document, so the spell checker uses the correct dictionary and can identify correctly spelled words in different languages.

(pause)

Vous vous dîtes, "mon dieu, nous avons tellement besoin de tests automatiques."

{me présenter}

Moi j'ai proposé une présentation sur Test Driven Development. Quand j'ai proposé ça à Gabriel, un des organisateurs du Meetup, m'a aussitôt répondu avec, et je répète ici le sens seulement, pas les mots exacts "C'est très intéressant.  Il y a beaucoup de projets plus vieux que TDD et qui ont de la difficulté à intégrer des tests puisque le travail à faire est massif après que le projet soit lancé."

(pause)

Mon vrai but est d'emmener la communité C++ à faire plus de tests unitaires.  Dans ma tête, ma vision était de présenter le dévelopement piloté par les tests comme tel, par le début.  On fait "test first".  Les tests sont unitaires, pas des tests d'intégration.  Le design du code vient de ce processus.  Mais il est évident que pour la plupart des gens ici, nous travaillons sur un projet qui n'a pas de tests, qui n'a pas le design approprié pour le tester.  La réalité est que les projets ne sont pas conçus pour cela.  J'ai donc aussitôt décidé de prendre la chose par l'autre côté.  Nous allons trouver comment injecter des tests automatiques dans vos projets.  Je ne vous parlerai pas de TDD, mais plutôt de tests automatiques.

Ce ne sera pas facile.  Il vous faudra probablement commettre des crimes horribles dans votre code pour y parvenir.  Honnêtement, je n'ai pas tellement d'expérience là dedans.  En fait, je n'ai fait que deux ou trois trucs du genre.  Je n'ai pas de réponses toutes faites pour votre situation particulière.  De toutes façons, je ne crois pas que quelqu'un ait une solution passe partout, pour tous les projets.

Alors, pour atteindre mon but, je vais vous infecter avec le virus des tests automatiques.  Je vais vous expliquer clairement ce que vous en retirerez.  Je vais vous donner des pistes pour intégrer des tests dans vos projets et le reste du chemin, vous devrez parcourir seul, puisque que ce chemin est nécessairement unique à chaque projet.

(pause)

Il y a de cela environ dix ans, mon patron, qui savait que j'écrivais des tests automatiques, me demande pourquoi, me "challenge" sur cette approche.  Je dois dire qu'à ce moment, je n'arrive pas à bien lui expliquer.  J'ai cafouillé une espèce de réponse et je n'ai pas eu l'impression de bien le convraincre.  Il m'a laissé continuer.  Il me faisait confiance j'imagine.

Mais aujourd'hui, je suis moi même bien infecté par les tests automatiques et j'ai beaucoup de bonnes raisons.


## Pourquoi TDD?

{Idée narrative: nous sommes programmeurs et nous sommes les premiers à comprendre que les ordinateurs peuvent nous libérer des tâches répétitives.  Alors, pourquoi répétez vous les mêmes étapes dans votre application durant le développement?}

{pour éviter de lancer l'app, clique clique clique pour atteindre la situation, les intrants nécessaires pour votre fonction qui est en developpement}

{pour pouvoir changer sans risques une fonction ou système qui est complexe et comporte bcp de cas et sous cas et qui est utilisée partout dans le programme.  je peux donner comme exemple fform}

{pour pouvoir mettre à jour une librairie externe et tout vérifier}

{pour pouvoir changer votre compilateur sans soucis}

{pour pouvoir tester plus d'une plateforme}

{pour réduire votre temps dev->production}

{pour pouvoir faire un dernier refactor sans peur}

{pour facilement s'assurer que la matrice de cas est toujours couverte}

{pour pouvoir avoir un meilleur design pls flexible}

{ça nous donnera des fonctions simples}

## Commencer

Mais par où commencer?  Vous êtes libres de choisir, mais je dirais que le mieux serait de commencer par votre prochaine tâche, que ce soit une nouvelles fonctionalité ou mieux, une correction de bogue.  

À mon emploi précédent, nous avions un format spécial pour les nombres.  C'était un logiciel de modélisation des transports, et ce logiciel produit *beaucoup* de nombres dans des tableaux et des graphiques.  C'est une fonction qui supporte des précisions dans la fraction mais aussi dans la partie entière.  On peut demander le format spécial ou le format scientifique classique.  Évidemment qu'un beau jour un bogue a été trouvé là dedans.

La fonction s'appelle `fform`.  Elle est très complexe, pleine de conditions, de calculs de longueur et de puissances en base dix.  Clairement, le moindre changement à `fform` allait briser quelque chose.

(pause)

C'est la situation parfaite pour commencer des tests automatisés. En plus, `fform` prend toutes ses entrées en paramètre et son seul résultat est une valeur de retour.

> {slide} Disons que le prototype de `fform` est le suivant:

```
string fform(double n, int precision, bool engineer);
```

Mais il nous manque un gros morceau.  Il nous manque un endroit pour exécuter nos tests.  Après la compilation du projet, nous devons pouvoir lancer les tests.  Cet exécutable doit nous donner au minimum soit "succès, tous les tests ont passés", soit "échec et voici quels tests ont échoués".

Le mieux est un nouvel exécutable ajouté à votre projet.  Pour ça, il faudra que ce nouvel exécutable ait accès à `fform`.  Si `fform` n'est pas déjà dans une librairie, il faudra trouver une solution.

Votre première option est de créer une librairie et déplacer `fform` dedans.  Vous ajoutez un nouveau target librairie avec `fform.cpp` dedans, vous enlevez `fform.cpp` du programme principal et vous linkez la librairie à ce programme.
>{slide} "nouvelle librairie"

- prendre tout le programme et le déplacer dans une librairie et renommer `main`.  Votre nouveau programme n'a qu'un tout petit `main` qui rebondi au `main` réel qui est dans la librairie.
> {slide} ajouter "le programme entier devient une librairie"
- ajouter une option à la ligne de commande, `--test` peut être, et diriger l'exécution vers une fonction `test()` très tôt dans `main`.  J'ai déjà fait cela pour tester des fonctions QML dans un projet Qt.  Certains tests d'éléments GUI étaient manuels, mais tout ce qui pouvait être automatisé l'était.
> {slide} ajouter "le programme devient deux programmes"
- le fichier source de `fform` va être compilé une deuxième fois pour exister dans le programme de test.
> {slide} ajouter "accès par le fichier source"

Maintenant, nous sommes prêt à écrire des tests.  Mais souvenez-vous, il nous faut un programme qui minimalement donne "succès" ou "échec au test X".  Vous pouvez bien sûr vous faire votre petit framework de test, mais ne le faites pas.  Choississez un framework existant.

Moi je vous recommande soit Catch2 ou Boost Unit Test.  Ces deux là ont toutes les fonctionnalités qu'il vous faudra:
> {slide}
> - bonne intégration à votre environnement
> - des "matchers",
> - nombre flottants,
> - exception lancées (lancée, type et contenu), 
> - fixtures, 
> - préférablement un style par expression

> {slide}
> TEST_CASE("fform") {
>   CHECK(fform(12.34, 1, false) == "12.3");
>   CHECK(fform(1234.5, -1, false) == "1230");
> }
> 
> TEST_CASE("fform notation ingénieur") {
>   CHECK(fform(12.34, 1, true) == "12.3");
>   CHECK(fform(123456, 0, true) == "123m456");
>   CHECK(fform(123456, 3, true) == "123m");
> }

Voici nos premiers tests avec le framework Catch2.

**{mon exemple avec fform n'est pas si hot que ça...}**

Tout ça a l'air plutôt anodin.  Votre patron, comme le mien, pourrait bien se demande, "est-ce vraiment utile?"  Et bien oui, c'est très utile!

Premièrement, nous avons maintenant une documentation sur le fonctionnement de `fform`.  Une documentation forcément correcte, maintenant et pour toujours.

Nous avons pas eu besoin de lancer l'application des dizaines de fois pour tester manuellement le changement.  Pas besoin d'un dataset spécifique, pas besoin de reproduire dans l'appli la situation particulière qui reproduit le problème.  C'est une énorme économie de temps.  

Quand on a changé le code, après nous pouvons être sûr que tous les autres cas n'ont pas étés brisés.  Il va être rare que l'équipe d'assurance qualité va revenir avec un nouveau problème.  Encore d'énormes économies de temps.

Et si le code de `fform` gagnerait à être amélioré?  Est-ce qu'il vous est déjà arrivé de laisser tomber un refactoring qui rendrait le code meilleur parce que l'équipe QA n'aura pas le temps de faire ou refaire tous les tests associés à ce code?  Eh bien maintenant, vous êtes libres d'enfin aller plus loin dans vos refactorings.

Ça paye d'écrire des tests.  Ce n'est pas gratuit.  C'est pas facile.  Mais ça paye.

(pause)

## User authentication

Cet exemple avec une fonction toute simple, sans effet de bord, autre dépendence ou entrée complexe est simple.  Ceux d'entre vous qui ont déjà voulu écrire des tests automatiques savent que quoi je parle et rongent leur frein depuis le début de la présentation! C'était juste pour commencer lentement.  Du vrai code en production est beaucoup plus complexe.  

Nous allons donc prendre un exemple plus réaliste.  Supposons que votre appli demande un login à l'utilisateur.  Quand l'appli démarre, si l'utilisateur ne s'est jamais connecté, on va demander nom et mot de passe, vérifier avec un serveur Internet avec une requête HTTP, et si c'est positif, sauvegarder le nom de l'utilisateur et le token de l'authentification dans un fichier.

Tout ça est fait dans la fonction `login`.  Dans le programme, `login` est appelée.  C'est une fonction sans arguments, qui retourne un code d'erreur dans un optional.

> {slide}
> LoginStatus login() {
>   optional\<Authentication> authentification = readAuthentification();
>   if( authentification ) return LoginStatus::LoggedIn;
>   
>   do {
>      optional\<Credentials> credentials = getCredentials();
>      if( !credentials ) return LoginStatus::Aborted;
>      
>      optional\<string> token = requestLogin(\*credentials);
>   } 
>   while( ! token );
>   
>   writeAuthentication(username, token);
>   return LoginStatus::LoggedIn;
> }

Comme vous pouvez voir, la gestion d'erreur domine la fonction.  J'ai mis la gestion d'erreur dans mon exemple parce que c'est surtout ça qui est à la fois difficile et intéressant à tester.  

Normalement, on utiliserait des exceptions je pense, mais la gestion d'erreur est faite avec un enum.  J'ai utilisé des enums pour qu'on puisse bien voir les erreurs parce qu'on doit les tester.

Alors, on a des optional et même des expected.  On aura expected dans C++23.  Pour ceux qui ne connaissent pas expected, ce type contient soit une valeur, soit une erreur.  


ça, elle va faire un accès disque et lire un fichier.  Déjà, ça commence à être difficile pour nos tests parce que si nous voulons tester `login` quand une authentification est présente, il nous faut un fichier au bon endroit avec le bon contenu.

Vous me direz que ce n'est si pire, écrire un petit fichier pour avoir un test.  Bon, comme le fichier en question est "production", ça va affecter votre appli normale qui est installée.  Peut être que vous aller devoir vous reconnecter avec un bon usager quand vous utiliserez l'appli normalement après avoir exécuté les tests.  Petit inconvénient, mais bon...

S'il n'y pas d'authentification présente, `login` va faire afficher un dialogue.  Un beau dialogue va apparaître et attendre un usager et mot de passe.  Ça ne sera pas un test très automatique.

Ce n'est pas terminé!  Après que nous avons l'usager et son passe, il faut faire une requête réseau.  Pour tester le succès, il faudra avoir cet usager avec ce passe sur le serveur.  J'espère pour vous que  ce n'est pas un problème et que vous avez une équipe qui peut vous aider rapidement.  

En plus de ça, une requête réseau peut de terminer de plusieurs façons.  Échec de connexion, "host name not found", "timeout".  Ce n'est pas facile à simuler en test automatique ça.  Encore plus difficile, de simuler le serveur qui donne une réponse du genre "internal error".   S'il y a une situation intéressante à tester, c'est bien celle là.  Personne autre que vous, le développeur, a la possibilité de tester ça efficacement.  Comment notre petit code de test ici va pouvoir dire au serveur "à la prochaine requête, retourne "internal error" "?

Ensuite la fonction `login` sauvegarde ou pas l'authentification, mais ce n'est plus ce qui nous intéresse.  Ce qui nous intéresse c'est de savoir comment nous allons tester ça.

{avec du logging?}









{où placer le code de test, deux méthodes: 1. les tests sont très près du code testé ou 2. les tests sont dans un projet externe.  Dans notre cas, il est probablement mieux que les tests soient proches}

{explorer comment exécuter les tests!!  CI?  }

## Exemples

{exemple avec authentification usager, Auth gateway}


## Situation 1

{Nouvelle fonctionalité: envoi de commentaires}

{qqpart la dedans se trouve une fonction de validation de courriel}

{en véritable TDD, c'est "tests first", donc c'est parfait, c'est comme cela que nous allons faire.}

{ajouter le projet de test, ajouter le cpp de test, écrire `#include "email.hpp"`, etc}

{écrire notre premier test}

{code un peu}

{écrire notre 2e test}

## Situation 2

{Bogue trouvé}


## Unitaire versus intégration

{le vrai problème, c'est que le code patrimonial (legacy) et couplé sur des choses difficiles à préparer.  Des macros fixtures.}

{macro fixtures ou fixtures physiques: DB, un serveur réseau, un autre processus, un fichier, la console, un UI}

{Aussitôt que notre code prend ses entrées de l'un de ces "fixtures" ou envoi directement vers, on est cuit}

{on tombe directement dans le test d'intégration}

{il y a deux problèmes: 1. pratiquement seul le happy path peut être testé: les cas vraiment exceptionels ne sont pas reproduisibles.  2. le coût de setup et le temps d'exécution des tests sont grands. }

{il vous faut changer votre code pour briser ces liens fixes et pouvoir remplacer ces macros fixtures}

{et oui, c'est bcp de travail et de risque, mais le jeu en vaut la chandelle.  vous en retirerez bien plus que ce que vous y metterez.  il deviendra possible de tester des situations exceptionnelles difficilement reproduisibles.  Tester des logiques de débordement de tableau.  Des situations rares ou par exemple un fichier dans lequel on doit écrire est bloqué; Des situations ou l'on a un comportement spécial quand un code d'erreur spécifique revient de l'OS.  Même jusqu'à simuler des situtations de timeouts.}

## Remplacer la dépendance par l'implantation

{en gros, l'idée est que si le code de la dépendance est dans une librairie ou un module logique, on conserve les headers et on fourni une nouvelle implantation, un fake.}

## Vous n'avez pas placé vos manipulations BD derrière une abstraction

{toutes vos manip DB sont à la main, hadoc, directement avec des appels à la librairie ODBC.}

{je n'ai rien pour vous.  il sera très difficile pour le fake, un fake de lib ODBC (!!!) de faire son travail}


## C'est quoi un fake

{un fake est un perroquet ou un espion, parfois les deux}

{c'est Néo dans la matrice, le code sous test est Néo, le fake est la matrice.}

{le code sous test interagit avec le fake}

{disons une classe d'authentification usager est sous test}

{donner des petits trucs au passage: red-green-refactor par exemple serait bien à placer qqpart dans la prezze}


## Finale

{qqchose de mémorable}

{ma conclusion serait "c'est pour toutes ces raisons que vous devriez dès demain faire des efforts pour introduire des tests dans ovs projets"}

{ma dernière phrase serait:}

Je terminerai avec cette question.  Alors, maintenant, qu'allez vous faire mercredi matin?

(pause)

Merci

----------------------------------------------

## Matériel

### Saboteur

pour tester le traitement d'erreur, j'ai ajouté à la classe sous test trois choses: `friend class BackdoorForTest`, un `std::function<void()>` et un appel à cette fonction à un point important pour les tests dans le code de la classe sous test.

En préparation pour le test, je peux alors, à l'aide d'une fonction `static` sur la classe `BackdoorForTest`, définie spécifiquement pour le test, injecter une fonction dans la `std::function`.

Nous pouvons donc simuler des conditions difficilement reproduisibles.

### ConfObj et Parameter

{faudra que je cherche pour rendre ce matériel utilisable}

Je voulais clarifier et travailler dans les signaux envoyés par Parameter.

```c++
// ...
void valueChanged(double newValue);
void valueChanged(double newValue, int index);
void valueChanged(double newValue, double oldValue);
void valueChanged(QString const & newValue);
void valueChanged(bool newValue)
void valueChanged(bool newValue, int index)
```

J'ai la classe Parameter.  Elle utilise quelques morceaux externes:
- 14 méthodes sur ConfObj
- Expression::compile()
- QTextStream Diag (logan.cpp)
- generateError() (main.cpp)

J'ai quelques méthodes que je voudrais désactiver.  Une technique serait de rendre des méthodes virtuelles et dériver de `Parameter` pour remplacer ces méthodes indésirables.


Pour ce faire, il me fallait donc créer ces objets, qui eux-même requieraient des `ConfObj`, qui eux-mêmes avaient besoin de l'objet applicatif, `Logan`.

### Login

Disons une fonction `std::optional<License> Login(string const & username, string const & password)`.  Cette fonction le type de licence si succès. `nullopt` en cas d'erreur avec l'authentifiant ou lance une exception en cas d'échec.  Cette fonction effectue donc toutes ces tâches:
- préparer un JSON
- placer une requête HTTP à un URL, avec le corps JSON
- détection et réaction à une erreur réseau
- détection et réaction à une erreur HTTP
- détection et réaction à une erreur provenance du serveur d'authentification
- décoder le JSON en réponse pour obtenir la licence
- détection et réaction à une erreur de décodage

Nous pouvons donc parler ici de comment nous pourrions tester ce code en séparant le code de requête HTTP d'avec le reste.  Comment injecter cette "interface.":
- Par callback
- Par lambda
- Par interface
- Par template, une ref à un type d'objet qui est un client HTTP
- Par remplacement du code
- Par Dependency Injection Container


----------------------------------------------


## Structure et technique de la pres

{faut pas trop être détaillé, ce n'est pas à l'écrit, ce sera difficile si je pousse chaque point et détail jusqu'au bout.   Autrement dit, ne cherche pas la perfection, cherche l'idéal pour la situation}

{je peux peut être compter sur la période de questions pour combler des omissions volontaires}

{2 à 5 points principaux, puis 2 à 5 affirmations par point}

{commencer et finir avec les points les plus forts}

----------------------------------------------

## Rebuts

Intro abandonnée:
Est-ce que vous aimez recevoir un ticket du groupe de test, concernant une feature que vous avez complété il y a deux semaines?  Vous êtes en plein dev d'une autre affaire, mais là il faut mettre ça sur pause pcq vous avez ce ticket?  Est-ce que aimez télécharger les fichiers nécessaires à la reproduction du bogue, lancer l'application, cliquer les boutons et naviguer les choix jusqu'à la reproduction?  Pour utiliser le déboggueur pour localiser le problème?
