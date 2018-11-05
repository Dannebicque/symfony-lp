# Modèles - Entités - ORM

* [Introduction](#introduction-1)
* [Mise en application](#mise-en-application)
* [Annotations](#annotations-1)
* [Modifications de champs et lien base de données](#modifications-de-champs-et-lien-base-de-données)
* [ORM](#orm-1)
* [Recherche d'entité](#recherche-dentité)
* [Exercice](#exo)

## Introduction

Dans Symfony la notion de modèle se retrouve sous la forme (entre autre) d'une **Entité**. Une entité est une **classe PHP**, qui **peut** être connectée à une table de votre base de données via l'ORM. Lorsqu'une entité est liée à une table, via l'ORM, il y a en général un fichier "repository" associé. Un repository permet la génération de requêtes simples ou complexes et dont le développeur peut modifier à volonté.

Un ORM (Object Relation Mapper) permet de gérer manipuler et de récupérer des tables de données de la même façon qu'un objet quelconque, donc en gardant le langage PHP. Plus besoin de requête MySQL, PostgresSQL ou autre.

Symfony utilise Doctrine comme ORM dans son système par défaut. Nous allons utiliser Doctrine mais vous pouvez utiliser d'autres systèmes si vous le souhaitez. Doctrine peut-être géré de plusieurs façon : XML, JSON, YAML, PHP et en Annotation nous allons utiliser ce dernier format de données.

[La documentation officielle de Symfony sur l'ORM Doctrine](https://symfony.com/doc/current/doctrine.html)
[La document officielle de Doctrine](https://www.doctrine-project.org/projects/orm.html)

Vous êtes libre d'écrire le code qui permet le traitement métiers en dehors des entités et d'avoir votre propre logique d'organisation. 

## Mise en application

### Configuration
Comme à chaque fois, il est d'abord nécessaire d'installer les bundles nécessaires pour manipuler la base de données avec un ORM.
Il vous faut donc executer la commande ci_dessous :

```
composer require symfony/orm-pack
```

On va également installer, si vous ne l'avez pas encore fait, le bundle "maker" qui contient des outils pour générer du code sous Symfony grâce à la console.

```
composer require symfony/maker-bundle --dev
```

Une fois ces deux éléments installés, il faut configurer la connexion à la base de données. Pour ce faire, il faut éditer le fichier .env à la rachine de votre projet, qui doit normalement contenir une ligne d'exemple.

```
# .env

# customize this line!
DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name"

# to use sqlite:
# DATABASE_URL="sqlite:///%kernel.project_dir%/var/app.db"
```

### Création de la base de données
Une fois le fichier à jour avec vos données, vous pouvez créer votre base de données depuis la console.
```
php bin/console doctrine:database:create
```

Les modificiations de structure de votre base de données devront être réalisées avec la console pour que Symfony puisse faire le lien entre les tables et l'ORM.

### Création d'une entité liée à une table

Utilisez la commande make:entity (qui est dans le bundle maker) pour avoir une série de question vous permettant de créer votre entité avec l'utilisation de l'ORM Doctrine. Vous pouvez créer une nouvelle entité ou modifier (ajouter des champs)  une entité déjà existante en saisissant son nom.

```
php bin/console make:entity
```

Vous allez devoir répondre à une suite de question avec le nom de l'entité (par défaut cela donnera le nom de la table), et les champs à créer.
Dans Symfony une entité possède toujours un champs id, qui est la clé primaire et qui est auto-incrémenté. Vous ne devez donc pas l'ajouter dans la console.

Pour la création d'un champs, il vous faudra donner :
* son type
* sa taille le cas échéant
* si ce champs peut être null
* s'il doit être unique (index)

Vous pouvez obtenir la liste des types supportés en tapant "?" à la question du type.

Une fois terminé, le fichier d'Entité et le repository associé sont générés.

Exemple dans la console :
```
php bin/console make:entity

Class name of the entity to create or update:
> Product

 to stop adding fields):
> name

Field type (enter ? to see all types) [string]:
> string

Field length [255]:
> 255

Can this field be null in the database (nullable) (yes/no) [no]:
> no

 to stop adding fields):
> price

Field type (enter ? to see all types) [string]:
> integer

Can this field be null in the database (nullable) (yes/no) [no]:
> no

 to stop adding fields):
>
(press enter again to finish)
```

Et le code de l'entité généré dans src/Entity/Product.php :

```
// src/Entity/Product.php
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity(repositoryClass="App\Repository\ProductRepository")
 */
class Product
{
    /**
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=255)
     */
    private $name;

    /**
     * @ORM\Column(type="integer")
     */
    private $price;

    public function getId()
    {
        return $this->id;
    }

    // ...Les getters et les setters sont automatiquement générés également. Par défaut il n'y a pas de constructeur. Vous pouvez en ajouter un si besoin.
}
```

A ce stade l'entité est créé, mais n'existe pas dans la base de données. Il reste deux étapes à exécuter.

La création d'un fichier de migration qui va contenir le code SQL a executer en fonction de votre SGBD.
```
 php bin/console make:migration
```

La mise à jour de votre base de données en fonction du fichier précédemment généré.
```
php bin/console doctrine:migrations:migrate
```

Si vous consultez votre PHPMyAdmin vous verrez la table apparaître.

## Modifications de champs et lien base de données

Pour modifier des champs vous pouvez éditer directement le code généré dans la partie annotation: nom (par défaut le nom de la variable), taille, type.

Pour ajouter des champs il vous faut relancer la commande make:entity en remettant le nom de votre entité.

Après chaque modification ou ajout il faut de nouveau générer le fichier de migration et mettre à jour la base de données. Vous pouvez bien sûr modifier ou créer plusieurs entités avant de faire une mise à jour de votre base de données.

## ORM


## Recherche d'entité


## Exercice
