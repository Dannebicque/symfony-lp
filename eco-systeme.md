# Eco-Système de Symfony

Symfony nécessite tout un environnement pour fonctionner. On a déjà vu Composer pour gérer les dépendances par exemple.

Symfony implique aussi différents "langages" et utilise un vocabulaire spécifique \(souvent repris dans d'autres framewrok\).

## Composer

**Composer** est un logiciel gestionnaire de dépendances libre écrit en PHP. Il permet à ses utilisateurs de déclarer et d'installer les bibliothèques dont le projet principal a besoin. Le développement a débuté en avril 2011 et a donné lieu à une première version sortie le 1 mars 2012. Développé au début par Nils Adermann et Jordi Boggiano  \(qui continuent encore aujourd'hui à le maintenir\).

Le logiciel Composer est fortement inspiré du logiciel [npm](https://fr.wikipedia.org/wiki/Npm_%28logiciel%29) pour [Node.js](https://fr.wikipedia.org/wiki/Node.js).

## ENTITÉ \(EQ. DU MODÈLE\)

Une entité est une classe PHP. Elle peut faire le lien avec une base de données, on y déclare les différentes propriétés accessibles; Symfony utilise par défaut un outil de persistence de données : Doctrine pour lier une entité à une table de base de données.

## ORM : OBJECT RELATIONNAL MAPPING

Système permettant de se libérer des requêtes pour la base de données. Il se charge de générer les requêtes à effectuer sur les Entités spécifiées.

## Repository

Classe PHP qui fait le pont entre une entité et l'ORM, il permet notamment de structurer des requêtes complexes.

## YAML

Format de structuration de données très utilisé dans Symfony, mais on peut utiliser du JSON, XML ou des classes PHP, les fichiers de configurations par défaut sont en YAML.

{% hint style="info" %}
Dans la version 6, cette syntaxe ne sera plus utilisée, et sera remplacée par du PHP
{% endhint %}

## ANNOTATION

Commentaire PHP directement dans les classes utiles \(controller, entité\) interprété par Symfony pour générer des fichiers de config temporaires; Nous utiliserons pour un soucis de simplification en majorité cette notation.

## Routes

Les routes permettent de faire un lien entre une URL et un contrôleur. 

## Bundles

Sorte de modules Symfony qui peuvent contenir tout et n'importe quoi ; C'est la force de Symfony les modules peuvent fonctionner indépendamment et même sur d'autres structures PHP, autre framework etc.

Cette notion a disparu avec la V4 de Symfony, mais il reste possible d'installer des Bundles tiers.

## ENVIRONNEMENTS

Symfony propose par défaut 2 environnements : dev et prod qui permettent de donner des configs différentes en fonction de l'environnement de travail ; dev permet une utilisation sans cache avec des outils de dev comme le profiler ; prod lui permet d'utiliser le site sous cache et sans aucun message d'erreurs. De plus on peut configurer les différentes environnements pour par exemple rediriger tous les mails vers toto@titi.com en dev et laisser le fonctionnement normal pour prod ; pratique pour les debugs.

Symfony propose également de définir autant d'environnement que nécessaire afin d'avoir différentes configurations. Le changement d'un environnement à un autre se faire en modifiant la ligne suivante dans le fichier ".env" :

```text
   ###> symfony/framework-bundle ###
   APP_ENV=dev
```

## Profiler

Le profiler est un outil puissant \(et indispensable\) pour débuger une application. Par défaut le profiler n'est pas installée. Pour l'ajouter il faut executer la commande suivante :

```text
composer require profiler --dev
```

Le profiler est toujours visible en bas de la page en mode développement.

