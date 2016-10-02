# Principes de conception

## Responsabilité unique

C'est sans doute le principe de conception logicielle le plus important. Une application bien conçue est décomposée en sous-parties. Selon la complexité de l'application, chaque sous-partie peut à son tour est décomposée en sous-parties, jusqu'à arriver à des sous-parties non décomposables. 

L'essentiel est que **chaque sous-partie de l'application ait une et une seule responsabilité**. Cela signifie que chaque sous-partie (chaque composant) est dédiée à un seul rôle et ne va jamais au-delà. Ce rôle est en lien avec la problématique dont s'occupe la sous-partie.

Exemples :

* Une sous-partie qui s'occupe des affichages à l'écran participe ne devrait pas comporter de traitements métier, ni de code en rapport avec l'accès aux données.
* Un composant de traitements métier (calcul scientifique ou financier, etc) ne doit pas s'intéresser ni à l'affichage des données qu'il manipule, ni à leur stockage.
* Une classe d'accès à une base de données (connexion, exécution de requêtes) ne devrait faire ni traitements métier, ni affichage des informations.

Le principe de responsabilité unique (*Single Responsibility Principle*) vise à construire une application de manière modulaire à partir d'éléments de base dont le rôle est bien identifié. Au moment où un nouveau besoin se fera sentir, il suffira d'intervenir sur la ou les sous-partie(s) concernée(s). Le reste de l'application sera inchangée : cela limite les tests à effectuer et le risque d'erreur.

## Encapsulation maximale

Ce principe de conception recommande de n'exposer au reste de l'application que le strict nécessaire pour que la sous-partie joue son rôle.

Au niveau d'une classe, cela consiste à ne donner le niveau d'accessibilité **publique** qu'à un nombre minimal de membres, qui seront le plus souvent des méthodes.

Au niveau d'une sous-partie d'application composée de plusieurs classes, cela consiste à rendre certaines classes **privées** afin d'interdire leur utilisation par le reste de l'application.

## Couplage faible

La définition du couplage est la suivante : "une entité (sous-partie, composant, classe, méthode) est **couplée** à une autre si elle dépend d'elle", autrement dit, si elle a besoin d'elle pour fonctionner. Plus une classe ou une méthode utilise d'autres classes comme classes de base, attributs, paramètres ou variables locales, plus son couplage avec ces classes augmente.

Au sein d'une application, un couplage fort tisse entre ses éléments des liens puissants qui la rend plus rigide à toute modification (on parle de "code spaghetti"). A l'inverse, un couplage faible permet une grande souplesse de mise à jour. Un élément peut être modifié (exemple : changement de la signature d'une méthode publique) en limitant ses impacts. 

Le couplage peut également désigner une dépendance envers une technologie ou un élément extérieur spécifique : un SGBD, un composant logiciel, etc.

Le principe de responsabilité unique permet de limiter le couplage au sein de l'application : chaque sous-partie a un rôle précis et n'a que des interactions limitées avec les autres sous-parties. Pour aller plus loin, il faut limiter le nombre de paramètres des méthodes et utiliser des **classes abstraites** ou des **interfaces** plutôt que des implémentations spécifiques ("Program to interfaces, not to implementations").

## Cohésion forte

Ce principe recommande de placer ensemble des éléments (composants, classes, méthodes) ayant des rôles similaires ou dédiés à une même problématique. Inversement, il déconseille de rassembler des éléments ayant des rôles différents.

Exemple : ajouter une méthode de calcul métier au sein d'un composant lié aux données (ou à la présentation) est contraire au principe de cohésion forte.

## DRY

DRY est l'acronyme de **Don't Repeat Yourself**. Ce principe vise à éviter la redondance au travers de l'ensemble de l'application. Cette redondance est en effet l'un des principaux ennemis du développeur. Elle a les conséquences néfastes suivantes :

* Augmentation du volume de code.
* Diminution de sa lisibilité.
* Risque d'apparition de bogues dûs à des modifications incomplètes.

La redondance peut se présenter à plusieurs endroits d'une application, parfois de manière inévitable (réutilisation d'un existant). Elle prend souvent la forme d'un ensemble de lignes de code dupliquées à plusieurs endroits pour répondre au même besoin, comme dans l'exemple suivant.

```
function A() {
  // ...
  
  // Code dupliqué
  
  // ...
}

function B() {
  // ...
  
  // Code dupliqué
  
  // ...
}
```

La solution classique consiste à factoriser les lignes auparavant dupliquées.

```
function C() {
  // Code auparavant dupliqué
}

function A() {
  // ...
  
  // Appel à C()
  
  // ...
}

function B() {
  // ...
  
  // Appel à C()
  
  // ...
}
```

Le principe DRY est important mais ne doit pas être appliqué de manière trop zélée. Vouloir absolument éliminer toute forme de redondance conduit parfois à créer des applications inutilement génériques et complexes. C'est l'objet des deux prochains principes.

## KISS

KISS est un autre acronyme signifiant **Keep It Simple, Stupid** et qu'on peut traduire par "Ne complique pas les choses". Ce principe vise à privilégier autant que possible la simplicité lors de la construction d'une application.

Il part du constat que la complexité entraîne des surcoûts de développement puis de maintenance, pour des gains parfois discutables. La complexité peut prendre la forme d'une architecture surdimensionnée par rapports aux besoins (*over-engineering*), ou de l'ajout de fonctionnalités secondaires ou non prioritaires.

Une autre manière d'exprimer ce principe consiste à affirmer qu'une application doit être créée selon l'ordre de priorité ci-dessous :

1. *Make it work*.
2. *Make it right*.
3. *Make it fast*.

## YAGNI

Ce troisième acronyme signifie **You Ain't Gonna Need It**. Corollaire du précédent, il consiste à ne pas se baser sur d'hypothétiques évolutions futures pour faire les choix du présent, au risque d'une complexification inutile (principe KISS). Il faut réaliser l'application au plus simple et en fonction des besoins actuels. 

Le moment venu, il sera toujours temps de procéder à des changements (refactorisation ou *refactoring*) pour que l'application réponde aux nouvelles exigences.