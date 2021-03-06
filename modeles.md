# Modèles - Entités - ORM

## Introduction

Dans Symfony la notion de modèle se retrouve sous la forme \(entre autre\) d'une **Entité**. Une entité est une **classe PHP**, qui **peut** être connectée à une table de votre base de données via l'ORM. Lorsqu'une entité est liée à une table, via l'ORM, il y a en général un fichier "repository" associé. Un repository permet la génération de requêtes simples ou complexes et dont le développeur peut modifier à volonté.

Un ORM \(**Object Relation Mapper**\) permet de gérer manipuler et de récupérer des tables de données de la même façon qu'un objet quelconque, donc en gardant le langage PHP. Plus besoin de requête MySQL, PostgresSQL ou autre.

Symfony utilise **Doctrine** comme ORM dans son système par défaut. Nous allons utiliser Doctrine mais vous pouvez utiliser d'autres systèmes si vous le souhaitez. Doctrine peut-être géré de plusieurs façon : XML, JSON, YAML, PHP et en Annotation nous allons utiliser ce dernier format de données.

[La documentation officielle de Symfony sur l'ORM Doctrine](https://symfony.com/doc/current/doctrine.html) [La document officielle de Doctrine](https://www.doctrine-project.org/projects/orm.html)

Vous êtes libre d'écrire le code qui permet le traitement métiers en dehors des entités et d'avoir votre propre logique d'organisation.

## Mise en application

### Configuration

Comme à chaque fois, il est d'abord nécessaire d'installer les bundles nécessaires pour manipuler la base de données avec un ORM. Il vous faut donc exécuter la commande ci\_dessous :

```bash
composer require symfony/orm-pack
```

On va également installer, si vous ne l'avez pas encore fait, le bundle "maker" qui contient des outils pour générer du code sous Symfony grâce à la console.

```bash
composer require symfony/maker-bundle --dev
```

Une fois ces deux éléments installés, il faut configurer la connexion à la base de données. Pour ce faire, il faut éditer le fichier `.env` à la racine de votre projet, qui doit normalement contenir une ligne d'exemple.

```text
# .env

# customize this line!
DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name"

# to use sqlite:
# DATABASE_URL="sqlite:///%kernel.project_dir%/var/app.db"
```

### Création de la base de données

Une fois le fichier à jour avec vos données, vous pouvez créer votre base de données depuis la console.

```bash
php bin/console doctrine:database:create
```

Les modifications de structure de votre base de données devront être réalisées avec la console pour que Symfony puisse faire le lien entre les tables et l'ORM.

### Création d'une entité liée à une table

Utilisez la commande `make:entity` \(qui est dans le bundle maker\) pour avoir une série de question vous permettant de créer votre entité avec l'utilisation de l'ORM Doctrine. Vous pouvez créer une nouvelle entité ou modifier \(ajouter des champs\) une entité déjà existante en saisissant son nom.

```bash
php bin/console make:entity
```

Vous allez devoir répondre à une suite de question avec le nom de l'entité \(par défaut cela donnera le nom de la table\), et les champs à créer. Dans Symfony une entité possède toujours un champs id, qui est la clé primaire et qui est auto-incrémenté. Vous ne devez donc pas l'ajouter dans la console.

Pour la création d'un champs, il vous faudra donner :

* son type
* sa taille le cas échéant
* si ce champs peut être null
* s'il doit être unique \(index\)

Vous pouvez obtenir la liste des types supportés en tapant "?" à la question du type.

Une fois terminé, le fichier d'Entité et le _repository_ associé sont générés.

Exemple dans la console :

```bash
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

Et le code de l'entité généré dans `src/Entity/Product.php` :

```php
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

La création d'un fichier de migration qui va contenir le code SQL a exécuter en fonction de votre SGBD.

```bash
 php bin/console make:migration
```

La mise à jour de votre base de données en fonction du fichier précédemment généré.

```bash
php bin/console doctrine:migrations:migrate
```

Si vous consultez votre PHPMyAdmin vous verrez la table apparaître.

## Modifications de champs et lien base de données

Pour modifier des champs vous pouvez éditer directement le code généré dans la partie annotation: nom \(par défaut le nom de la variable\), taille, type.

Pour ajouter des champs il vous faut relancer la commande `make:entity` en remettant le nom de votre entité.

Après chaque modification ou ajout il faut de nouveau générer le fichier de migration et mettre à jour la base de données. Vous pouvez bien sûr modifier ou créer plusieurs entités avant de faire une mise à jour de votre base de données.

## ORM

Une fois la base de données mise en place on va pouvoir insérer, modifier, supprimer et récupérer des informations de la base de données sans saisir de requêtes via des méthodes en initialisant l'entité fraichement créée :

```php
/**
 * @Route("/est", name="test")
 */
public function test()
{
    $post = new Post(); // initialise l'entité
    $post->setTitle('Mon titre'); // on set les différents champs
    $post->setEnable(true);
    $post->setDateCreated(new \Datetime);

    $em = $this->getDoctrine()->getManager(); // on récupère le gestionnaire d'entité
    $em->persist( $post ); // on déclare une modification de type persist et la génération des différents liens entre entité
    $em->flush(); // on effectue les différentes modifications sur la base de données 
    // réelle

    return new Response('Sauvegarde OK sur : ' . $post->getId() );
}
```

Il existe à la place de `$em->persist, $em->remove($post);` qui permettra de faire une suppression.

Ce dernier code effectue une création dans la base de données; pour une modification il suffit de modifier l'instanciation de l'entité de la sorte :

```php
/**
 * @Route("/test/modification", name="test")
 */
public function testModification()
{
    $post = $this->getDoctrine()->getRepository(Post::class)->find( 1 ); // récupération du post avec id 1 
    $post->setTitle('Mon titre'); // on set les différents champs
    $post->setEnable(true);
    $post->setDateCreated(new \Datetime);

    $em = $this->getDoctrine()->getManager(); // on récupère le gestionnaire d'entité
    $em->flush(); // on effectue les différentes modifications sur la base de données 
    // réelle

    return new Response('Sauvegarde OK sur : ' . $post->getId() );
}
```

ici on récupère le _repository_ de Post et on récupère l'id 1 ; tout le restant du code reste inchangé.

## Recherche d'entité

Symfony et Doctrine proposes des requêtes prédéfinies, qui répondent aux usages les plus courant.

Si `$em` est le manager associé à une entité :

* `$em->find( $id );` on récupère qu'un seul élément de l'entité avec l'id `$id`;
* `$em->findAll();` on récupère toutes les entrées de l'entité concernée
* `$em->findBy( $where, $order, $limit, $offset );` on recherche avec le tableau `$where` on tri avec le tableau `$order` on récupère `$limit` éléments à partir de l'élément `$offset`.
* `$em->findOneBy($where,$order);` on récupère le premier élément respectant le tableau `$where` et trié avec le tableau `$order`;
* `$em->findByX($search);` requêtes magiques où X correspond à n'importe quel champs défini dans votre entité
* `$em->findOneByX($search)` ; requêtes magiques où X correspond à n'importe quel champs défini dans votre entité

  Par exemple `findBySlug('home')`; ou `findByTitle('Bonjour);` génèrera des requêtes de recherche automatiquement. Pour les requêtes avec plusieurs éléments il faudra faire une itération \(foreach\) ou lister les différents éléments.

Exemple

```php
// Modifications multiples : 
/**
 * @Route("/est", name="test")
 */
public function test()
{
    $posts = $this->getDoctrine()->getRepository(Post::class)->findAll(); // récupération de tous les posts
    $em = $this->getDoctrine()->getManager(); // on récupère le gestionnaire d'entité

    foreach($posts as $post)
    {
        $post->setTitle('Mon titre' . $post->getId() ); // on set les différents champs
        $em->persist( $post ); // on déclare une modification de type persist et la
        // génération des différents liens entre entité 
    }

    $em->flush(); // on effectue les différentes modifications sur la base de données 
    // réelle

    return new Response('Sauvegarde OK ');
}
```

Si aucune requête prédéfinie ne correspond à vos besoin, vous pouvez bien sûr en créer une en passant par le _repository_.

Vous pouvez également générer vos requêtes manuellement pour avoir une requête complexe et précise directement dans le _controller_ mais idéalement il faudrait le placer dans le _repository_ dédié.

```php
// src/AppBundle/Repository/Post.php

public function maRequete( $where )
{
    // avec querybuilder
    $queryBuilder = $this->createQueryBuilder("p");

    $queryBuilder->where(' p.title like :w');
    $queryBuilder->setParameter(':w', '%'.$where.'%');
    $query = $queryBuilder->getQuery(); // on récupère la requêtes 

       return $query->getResult(); // on renvoie le résultat
}
//OU
 public function maRequeteSQL( $where )
    {
        // avec requête SQL
        $em = $this->getEntityManager();
        $query = $em->createQuery('SELECT p from AppBundle:Post p 
    WHERE p.title like :w');

        $query->setParameter(':w', '%'.$where.'%');


        return $query->getResult(); // on renvoie le résultat
     }
}
```

Et l'utiliser dans votre _controller_

```php
// src/AppBundle/Controller/DefautController
$this->getDoctrine()->getRepository(Post::class)->maRequete('test');
```

## Exercice

* Créer une entité "Post" avec :
  * title string 255
  * dateCreated datetime
  * content text
  * enable boolean
* Créer une entité "PostCategory" avec :
  * title string 255
* Créer une page qui va sauvegarder une catégorie avec le nom "Catégorie 1".
* Créer une page qui va sauvegarder un post avec le nom Post 1 à la date courante avec comme contenu Lorem ipsum et en enable à true. 
* Créer une page qui va afficher le titre de la catégorie en id 1 et le post en id 1.
* Créer un nouveau post identique au premier en changeant le titre.
* Créer une page qui affiche la totalité des entités Post. 
* Créer une page qui récupère le Post avec le Titre "Post 1"

