# Sécurité

* [Authentification - Authorization](#introduction)
* [Configuration](#configuration)
* [Exercice](#exercice)


## Introduction

Deux notions majeures interviennent dans la conception de sécurité de Symfony :

* Authentification : Qui êtes vous ? ; vous pouvez vous authentifier de plusieurs manières (HTTP authentification, certificat, formulaire de login, API, OAuth etc)
* Authorization : Avez vous accès à ? ; permet d'autoriser de faire telle ou telle action ou accéder à telle page sans forcément savoir qui vous êtes, utilisateur anonyme par exemple.

Pour fonctionner, il est nécessaire d'ajouter le composant security à votre symfony.

````
composer require symfony/security-bundle
````

La sécurité dans symfony implique plusieurs éléments :

* Le firewall: qui est la porte d'entrée pour le système d'authentification, on définit différents firewall (au minimum 1 seul) qui va permettre de mettre en place le bon système de connexion pour l'url spécifiée via un pattern.
* Le provider : qui permet au firewall d'interroger une collection d'utilisateurs/mot de passe ; C'est une sorte de base de tous les utilisateurs avec les mots de passe. Il existe deux type par défaut :
  * in memory : directement dans le fichier security.yml mais du coup les hash des mots de passes sont disponible dans un fichier 
  * Entity : N'importe quelle entité qui implémente à minima Symfony\Component\Security\Core\User\UserInterface
  * Enfin, plusieurs providers peuvent fonctionner en même temps par exemple in_memory et entity voire plusieurs entités simultanément. http://symfony.com/doc/current/security/entity_provider.html
* Un encoder : qui permet de générer des hashs/d'encoder des mots de passe ; le plus connu étant MD5 mais vous pouvez utiliser d'autres encoders tels que : sha1, bcrypt ou plaintext (qui n'encode rien c'est le mot de passe en clair) http://symfony.com/doc/current/security/named_encoders.html
* Les rôles : qui permettent de définir le niveau d'accès des utilisateurs connectés (authentifiés) et de configurer le firewall en fonction de ces rôles. Les rôles peuvent être hierarchisées afin d'expliquer par exemple qu'un administrateur (ROLE_ADMIN par exemple) et avant tout un utilisateur (ROLE_USER).

## Configuration

A partir de la version 4, et avec le composant "maker", la gestion de la sécurité a été grandement facilitée. Là où sur les précédentes versions (la 2 notamment), il était d'usage de passer par un bundle tierce (FOSUserBundle par exemple), aujourd'hui cela n'est plus nécessaire.

### Créer sa classe User

Si vous ne disposez pas encore d'une classe permettant la gestion des utilisateurs, il est possible d'en créer une avec la consôle. Si vous disposez déjà d'une classe utilisateur (ou que vous souhaitez utiliser plusieurs entités, il faudra modifier votre code en implémentant l'interface UserInterface et en respectant les conventions imposées).

L'instruction ci-dessous permet de lancer la consôle pour créer la table User.

````
bin/console make:user
````

Symfony va vous poser plusieurs questions afin de configurer les éléments (le nom de l'entité, si vous utilisez doctrine, le champ correspondant au login, et l'encodage du password.

````
The name of the security user class (e.g. User) [User]:
> User

Do you want to store user data in the database (via Doctrine)? (yes/no) [yes]:
> yes

Enter a property name that will be the unique "display" name for the user (e.g.
email, username, uuid [email]
> email

Does this app need to hash/check user passwords? (yes/no) [yes]:
> yes

created: src/Entity/User.php
created: src/Repository/UserRepository.php
updated: src/Entity/User.php
updated: config/packages/security.yaml
````

Une fois cette commande executée vous avez un fichier d'entité de créé, un repository associé, et le fichier security.yaml (dans config) qui a été mis à jour.

Il faut ensuite mettre à jour votre base de données, avec les commandes suivantes:

````
bin/console make:migration
bin/console doctrine:migrations:migrate
````

### Partie "User Provider"

Le User Provider permet d'assurer quelques tâches nécessaires pour la sécurité. Notamment le fait de récupérer le fait qu'un utilisateur soit connecté (lire la session par exemple), gérer le "se souvenir de moi", ...
Cette partie est configurée par défaut dans le fichier security.yaml. Elle peut être suffisante dans de nombreux cas et notamment quand la gestion se fait par une entité.

````
providers:
  user:
     entity: { class: App\Entity\User, property: username }
````

### L'encodage du mot de passe.

Là aussi, tout est pré-confiuré, vous pouvez bien sûr adapter. L'encodage permet de définir le "format" de cryptage du mot de passe.

````
encoders:
    # use your user class name here
    App\Entity\User:
        # bcrypt or argon2i are recommended
        # argon2i is more secure, but requires PHP 7.2 or the Sodium extension
        algorithm: bcrypt
        cost: 12
````            


### L'authentification et le firewall

C'est la partie essentielle du process de sécurisation. C'est lui qui permet de dire quand il faut vérifier et authentifier un utilisateur. Le firewall permet de déterminer pour un pattern d'url (une requête (request)), la méthode d'authentification à utiliser (une page de connexion, une clé d'API, une dépendance à un fournisseur OAuth, ...).

````
firewalls:
    dev:
        pattern: ^/(_(profiler|wdt)|css|images|js)/
        security: false
    main:
        anonymous: ~
````

L'exemple ci-dessus permet de définir que pour les routes particulières (les assets, le profiler), il n'y a pas de vérification. Pour toutes les autres routes (main), les "anonymes" sont autorisées (c'est à dire les personnes non authentifiées).

Symfony propose des exemples pour de nombreuses méthodes d'authentification (login, ldap, json, ...) que vous [trouverez sur la documentation officielle](https://symfony.com/doc/current/security/auth_providers.html)

### La gestion des rôles

La gestion des roles se fait dans la partie "access_control" du fichier security. Il permet de définir pour chaque pattern d'URL quel rôle peut y accèder.

Exemple:

````
access_control:
  # require ROLE_ADMIN for /admin*
  - { path: ^/admin, roles: ROLE_ADMIN }

ou

access_control:
  # matches /admin/users/*
  - { path: ^/admin/users, roles: ROLE_SUPER_ADMIN }

  # matches /admin/* except for anything matching the above rule
  - { path: ^/admin, roles: ROLE_ADMIN }
````

De cette manière les URL seront automatiquement bloquées si l'utilisateur ne dispose pas du bon rôle. Il est aussi possible de tester ce rôle directement dans un contrôleur ou dans une vue selon les besoins. [Voir la documentation pour plus d'éléments sur ce point](https://symfony.com/doc/current/security.html#securing-controllers-and-other-code)

### Récupérer l'utilisateur connecté

Enfin, il est souvent nécessaire de récupérer les informations sur l'utilisateur connecté. Pour cela, dans un contrô
leur il est possible d'utiliser directement l'instruction :

````
$user = $this->getUser();
````

### Gérer la déconnexion

Sur la même idée que pour la connexion, il est possible de gérer la déconnexion. Pour cela, dans le fichier security.yaml, il faut définir le path (la route) pour la méthode qui gére la déconnexion, et la cible (une route, optionnelle), une fois la déconnexion réussie.

````
firewalls:
     main:
         # ...
         logout:
             path:   app_logout
             # where to redirect after logout
             # target: app_any_route
````

La méthode dans le contrôleur peut se résumer à :

````
/**
 * @Route("/deconnexion", name="app_logout")
 */
public function logout(): void
{
}
````

Cette méthode qui ne retourne rien, permet la déconnexion, et la redirection se fait via le target définit dans security.yaml.

## Exercice

Mettre en place une classe User, et créer le formulaire de connexion en suivant la documentation. Filtrer la partie création de catégorie uniquement à des utilisateurs ayant le rôle ROLE_ADMIN.
