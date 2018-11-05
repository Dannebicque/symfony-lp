# Introduction

* [Présentation](#presentation)
* [Prérequis](#prérequis)
* [Organisation](#objectif-de-ce-cours)
* [Installation](#installation)

## Présentation

Découvrir et appréhender un framework PHP web.

## Pré-requis

* PHP
* Programmation Orientée Objet
* Structure MVC
* Base de données

## Organisation
* 20 heures de TD
* 12 heures de TP
* Notes :
    * 1 évaluation écrite (dernier TD)
    * 1 note pratique (lors du dernier TP)
    
## Installations

### Configuration requise pour votre serveur

* Un serveur Web
* PHP 7.1 ou supérieur
* [Git (différent de GitHub)](https://git-scm.com/)
* Le gestionnaire de dépendance [Composer (pour executer FLEX)](https://getcomposer.org/)
* Une maîtrise de son système d'exploitation ! (fichiers cachés, variables PATH, php.ini, console...)

### Installation des outils

* Installer un serveur local (si ce n'est pas déjà fait)
* Installer [Git](https://git-scm.com/)
* Installer [Composer](https://getcomposer.org/)
* Installer un vraie IDE ! (PhpStorm / NetBeans, ou éventuellement VSCode).

Pensez à vérifier que tout fonctionne correctement :

```
php -v
composer -v
git -v
```
Vous devez vous afficher les numéros de version. Corrigez les messages d'erreurs éventuels.

### Installation de Symfony.

Symfony propose deux versions :
* La version **skeleton**, la plus minimaliste et légere, qui n'installe que le stricte minimum, vous laissant ainsi la liberté d'ajouter les composants dont vous avez réellement besoin.
* Le version **website-skeleton**, qui va installer tout le nécessaire pour faire foncitonner un site (vues, annotations, base de données, ...)

On va privilgier la version **skeleton**.

Placez-vous dans le repertoire où vous souhaitez installer Symfony (www, public_html, ...) et exécutez la commande suivante (cela va créer un répertoire du nom de votre projet).

```
composer create-project symfony/skeleton nomDuProjet
```

Par défaut cette commande récupère la dernière version stable de symfony.
Le téléchargement peut prendre quelques minutes.

Il est possible de tester immédiatement le bon fonctionnement de l'installation en utilisant la ligne de commande en php suivante :

```
cd nomDuProjet
php -S 127.0.0.1:8000 public/index.php
```

Si vous vous rendez sur l'URL http://127.0.0.1:8000 vous devriez voir la page d'accueil de Symfony avec le numéro de la version installée.


Il est également possible d'installer un serveur embarqué avec Symfony. Cela peut servir pour des tests rapides.
Pour cela il faut installer ce serveur (la version skeleton ne contenant que le strict minimum)

```
cd nomDuProjet
composer require symfony/web-server-bundle --dev
```

Et l'executer :

```
 php bin/console server:run
```

Et vous trouverez par défaut, votre site à l'URL : http://localhost:8000/

