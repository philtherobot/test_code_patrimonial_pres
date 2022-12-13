échéancier
| x | pts | #sems |
|--|--|--|
| squelette | 1 | 1 |
| texte complet et détaillé | 5 | 5 |
| slides | 3 | 3 |
| apprendre/répéter | 2 | 2 |

C'est mercredi matin, la semaine est bien en route et votre prochaine feature avance bien.  Vous avez hâte de continuer.  Mais dans votre boîte de courriel se trouve une mauvaise nouvelle.  Assurance qualité vient de trouver un bogue avec votre feature de la semaine passée.  Vous allez devoir, un moment donné cette semaine, laisser votre travail ce côté.  Obtenir une branche *clean* dans un autre répertoire.  Télécharger les fichiers ou autres données qui sont en cause dans le ticket, compiler, lancer l'appli, suivre attentivement les étapes du repro, et observer le problème.  Débogguer.  Corriger (le bout le fun dans le fond). Se rendre compte qu'il y a deux autres endroits qui sont affectés et que vous seriez mieux de tester ça vous même avant de juste retourner ça en test.  Ou au moins expliquer à assurance qualité "by the way, faites ci et ça en plus durant les test pcq j'ai bidouillé ce coin là aussi".  Vous vous dîtes, "mon dieu, nous avons tellement besoin de tests automatiques."

{me présenter}

{histoire de 1er contact avec Gabriel}

Quand j'ai proposé une présentation sur TDD, Gabriel, un des organisateurs du Meetup de ce soir, m'a aussitôt répondu avec, et je répète ici le sens seulement, pas les mots exacts "C'est très intéressant.  Il y a beaucoup de projets plus vieux que TDD et qui ont de la difficulté à intégrer TDD puisque le travail à faire est massif après que le projet soit lancé."  

(pause)

Dans ma tête, mon plan quand j'ai proposé une présentation sur TDD était de présenter TDD comme tel, par le début.  On fait "test first".  Les tests sont unitaires, pas des tests d'intégration.  Le design du code.  Mais il est évident que pour la plupart des gens ici, nous travaillons sur un projet qui n'a pas de tests, qui n'a pas le design approprié pour le tester.  Mon vrai but est d'emmener la communité C++ à faire plus de tests unitaires.  La réalité est que les projets ne sont pas conçus pour cela.  J'ai donc aussitôt décidé de prendre la chose par l'autre côté.  Nous allons trouver comment injecter des tests dans vos projets.  

Ce ne sera pas facile.  Il vous faudra probablement commettre des crimes horribles dans votre code pour y parvenir.  En plus, honnêtement, je n'ai pas tellement d'expérience là dedans.  En fait, je n'ai fait que deux ou trois trucs du genre.  Je n'ai pas de réponses toutes faites pour votre situation particulière.  De toutes façons, je ne crois pas que quelqu'un ait une solution passe partout, pour tous les projets.

Alors, pour atteindre mon but, je vais vous infecter avec le virus du TDD.  Je vais vous expliquer clairement ce que vous en retirerez.  Je vais vous donner des pistes pour intégrer des tests dans vos projets et le reste du chemin, vous devrez parcourir seul, puisque que ce chemin est nécessairment unique à chaque projet.

Il y a de cela environ dix ans, mon patron, qui savait déjà que j'écrivais des tests automatiques, me demande pourquoi, me "challenge" sur cette approche.  Je dois dire qu'à ce moment, je n'arrive pas à bien lui expliquer.  J'ai cafouillé une espèce de réponse, et  j'ai continué mes tests, même si je n'ai pas eu l'impression de bien le convraincre.

Mais aujourd'hui, je suis moi même bien infecté par TDD et j'ai beaucoup de bonnes raisons.

{point 1: fonctions simples}
{exemple avec fform(double v, decimales, significatif, eng/scien) }
{outils: Catch2, Boost Test}
{refactoriser sans peur}
{pour réduire votre temps dev->production}
{pour facilement s'assurer que la matrice de cas est toujours couverte}

{point 2: authentification usager}
{exemple avec Auth gateway}

## Pourquoi TDD?

{JE NE SUIS PAS SATISFAIT DE CETTE APPROCHE: JE VOUDRAIS PLUS RACONTER DES HISTOIRES QUI MENENT AUX "POURQUOI"}

{Idée narrative: nous sommes programmeurs et nous sommes les premiers à comprendre que les ordinateurs peuvent nous libérer des tâches répétitives.  Alors, pourquoi répétez vous les mêmes étapes dans votre application durant le développement?}

{pour éviter de lancer l'app, clique clique clique pour atteindre la situation, les intrants nécessaires pour votre fonction qui est en developpement}

{pour pouvoir changer sans risques une fonction ou système qui est complexe et comporte bcp de cas et sous cas et qui est utilisée partout dans le programme.  je peux donner comme exemple fform}

{pour pouvoir mettre à jour une librairie externe et tout vérifier}

{pour pouvoir changer votre compilateur sans soucis}

{pour pouvoir tester plus d'une plateforme}

{pour réduire votre temps dev->production}

{pour pouvoir faire un dernier refactor sans peur}

{pour facilement s'assurer que la matrice de cas est toujours couverte}

{pour pouvoir avoir un meilleur design plus flexible}

## Commencer

{par les cas faciles, tout ce qui est fonction ou objet fermé, intrants->sortie}

{deux méthodes: 1. les tests sont très près du code testé ou 2. les tests sont dans un projet externe.  Dans notre cas, il est probablement mieux que les tests soient proches}

{choix d'une techno: Catch2 et Boost UnitTest. nous voulons une bonne intégration à votre environnement, des "matchers", nombre flottants, exception lancées (lancée, type et contenu), fixtures, préférablement un style par expression}

{il vous faudra au moins une librairie!  Le code de test est lui-même un exécutable, alors si votre appli est monolithique, il sera difficile d'avoir accès aux fonctions.}

{facile: changez le projet de l'appli de "exécutable" à "librarie", votre appli est un main.cpp et appelle `mymain` dans votre nouvelle lib}

{plus facile encore, si vous testez une fonction ou des fonctions d'un cpp qui se compile indépendemment du reste, votre target de test compilera le source sous test directement, pour lui même.}

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

{et oui, c'est bcp de travail et de risque, mais le jeu en vaut la chandelle.  vous en retirerez bien plus que ce que vous y mettrerai.  il deviendra possible de tester des situations exceptionnelles difficilement reproduisibles.  Tester des logiques de débordement de tableau.  Des situations rares ou par exemple un fichier dans lequel on doit écrire est bloqué; Des situations ou l'on a un comportement spécial quand un code d'erreur spécifique revient de l'OS.  Même jusqu'à simuler des situtations de timeouts.}

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


## Structure et technique de la prezze

{faut pas trop être détaillé, ce n'est pas à l'écrit, ce sera difficile si je pousse chaque point et détail jusqu'au bout.   Autrement dit, ne cherche pas la perfection, cherche l'idéal pour la situation}

{je peux peut être compte sur la période de questions pour combler des omissions volontaires}

{2 à 5 points principaux, puis 2 à 5 affirmations par point}

{commencer et finir avec les points les plus forts}

----------------------------------------------

## Rebuts

Intro abandonnée:
  Est-ce que vous aimez recevoir un ticket du groupe de test, concernant une feature que vous avez complété il y a deux semaines?  Vous êtes en plein dev d'une autre affaire, mais là il faut mettre ça sur pause pcq vous avez ce ticket?  Est-ce que aimez télécharger les fichiers nécessaires à la reproduction du bogue, lancer l'application, cliquer les boutons et naviguer les choix jusqu'à la reproduction?  Pour utiliser le déboggueur pour localiser le problème?
