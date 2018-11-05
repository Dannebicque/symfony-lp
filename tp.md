# TP

* [Présentation](#prsentation)
* [Notation](#notation)
* [TP1](#tp-1)
* [TP2](#tp-2)
* [TP3](#tp-3)

## Présentation

Réaliser un forum, en utilisant Symfony. Votre forum devra intégrer les fonctionnalités suivantes :

* Inscription, connexion, déconnexion
* Création de message dans les catégories du forum
* Possibilité de répondre à un message
* Une recherche simple
* Administration : Création des catégories et sous-catégories, administration des messages.
* Il y aura 3 niveaux d'accès : Administrateur, modérateur (possibilité de supprimer ou déplacer un message), membre (possibilité de créer un message)
* Un message pourra intégrer des fichiers

## Notation

L'esthétique du forum n'est pas prise en compte. L'usage d'une librairie CSS ou d'un template est suffisant.

**Par contre, vous veillerez à l'ergonomie et à la lisibilité.**

Le respect des consignes peut vous apporter jusque 15 points.
Les 5 points supplémentaires seront acquis en fonction des ajouts (pertinents) que vous ferez, soit pour proposer des fonctionnalités pertinentes, soit dans la qualité de la navigation et de l'accessibilité.

Le travail est individuel, et sera évalué en direct lors de la dernière séance de TP prévue le : 21/12 à 10h00.

## TP 1

Pour cette première séance vous devrez mettre en place les éléments suivants :

* Une nouvelle installation de Symfony (4.1 minimum)
* Mettre en place les controllers et les vues nécessaires à la navigation "publique" du site (on ne traitera pas la partie back-office aujourd'hui)
    * Les vues seront des prototypes, n'hésitez pas à mettre des données ficitives pour les rempalcer ensuite par les données issues de la base de données.
    * Ces données pourraient venir du controller de manière fictive.
* Intégrer un template ou une librairie CSS et faire un minimum de mise en page
* Reflechir au MCD que vous aller mettre en place.

## TP 2

* Mettre en place la base de données, les tables et les relations.
* Intégrer des données ficitives et modifier vos controllers pour alimenter vos vues avec ces données
* Intégrer les formulaires et la gestion des messages sur la partie publique.

## TP 3

* Mettre en place l'administration et les accès sécurisés.
* Mettre en place les CRUD pour le back-office.
* Intégrer la gestion des fichiers, par exemple avec le Bundle VichUploader.