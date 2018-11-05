# Eco-Système de Symfony

* [Composer](#composer)
* [MVC](#mvc)
* [Entité](#entité)
* [ORM](#orm)
* [Repository](#repository)
* [YAML](#yaml)
* [Annotation](#annotation)
* [Route](#route)
* [Bundle](#bundle)
* [Environnement](#environnement)
* [Profiler](#profiler)
* [Arborescence](#arborescence)
* [Lancement de l'application](#lancement-de-lapplication)
* [Exo 1](#exo-1)

Symfony nécessite tout un environnement pour fonctionner. On a déjà vu Composer pour gérer les dépendances par exemple.

Symfony implique aussi différents "langages" et utilise un vocabulaire spécifique (souvent repris dans d'autres framewrok).

## Composer


## MVC

## ENTITÉ (EQ. DU MODÈLE)
   Une entité est une classe PHP. Elle peut faire le lien avec une base de données, on y déclare les différentes propriétés accessibles; Symfony utilise par défaut un outil de persistence de données : Doctrine pour lier une entité à une table de base de données.

## ORM : OBJECT RELATIONNAL MAPPING
Système permettant de se libérer des requêtes pour la base de données. Il se charge de générer les requêtes à effectuer sur les Entités spécifiées.

## Repository
Classe PHP qui fait le pont entre une entité et l'ORM, il permet notamment de structurer des requêtes complexes.

## YAML
Format de structuration de données très utilisé dans Symfony, mais on peut utiliser du JSON, XML ou des classes PHP, les fichiers de config par défaut sont en YAML.

## ANNOTATION
Commentaire PHP directement dans les classes utiles (controller, entité) interprété par Symfony pour générer des fichiers de config temporaires; Nous utiliserons pour un soucis de simplification en majorité cette notation.

## Routes

## Bundles

Sorte de modules Symfony qui peuvent contenir tout et n'importe quoi ; C'est la force de Symfony les modules peuvent fonctionner indépendemment et même sur d'autres structures PHP, autre framework etc.

Cette notion a disparu avec la V4 de Symfony, mais il reste possible d'installer des Bundles tiers.

## ENVIRONNEMENTS
   Symfony propose par défaut 2 environnements : dev et prod qui permettent de donner des configs différentes en fonction de l'environnement de travail ; dev permet une utilisation sans cache avec des outils de dev comme le profiler ; prod lui permet d'utiliser le site sous cache et sans aucun message d'erreurs. De plus on peut configurer les différentes environnements pour par exemple rediriger tous les mails vers toto@titi.com en dev et laisser le fonctionnement normal pour prod ; pratique pour les debugs.
   
   Symfony propose également de définir autant d'environnement que nécessaire afin d'avoir différentes configurations. Le changement d'un environnement à un autre se faire en modifiant la ligne suivante dans le fichier ".env" :
   
   `````
   ###> symfony/framework-bundle ###
   APP_ENV=dev