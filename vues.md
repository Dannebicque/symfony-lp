# Vues - TWIG

* [Présentation](vues.md#affichage)
* [Exemple](vues.md#exemple)
* [Affichage](vues.md#affichage)
* [Variables](vues.md#variables)
* [Logique](vues.md#logique)
* [Tests](vues.md#tests)
* [Boucles](vues.md#boucles)
* [Héritage](vues.md#héritage)
* [Exos 1](vues.md#exos-1)
* [Inclusions](vues.md#inclusions)
* [Filtres](vues.md#filtres)
* [Assets](vues.md#assets)
* [Exercice 2](vues.md#exercice-2)

## Présentation

Twig est un moteur de rendu de template comme Smarty ou Blade \(laravel\), mais a des rapports très proches avec Symfony, Sensio a contribué énormément à son développement. Un moteur de template permet de limiter les logiques complexes pour réaliser des templates simples à coder. Un moteur de template intégre généralement des fonctionnalités qui sont récurrentes dans le développement "front" et qui permettent de simplifier le code à écrire.

**La syntaxe commence toujours avec {} des accolades.**

[La documentation officielle de Symfony sur les vues et TWIG](https://symfony.com/doc/current/templating.html) et [La documentation officielle de TWIG](http://twig.sensiolabs.org/)

## Interêt d'une vue

Exemple d'un code que vous pourriez écrire en PHP

```text
<body>
    <h1><?php echo $page_title ?></h1>
    <ul id="navigation">
        <?php foreach ($navigation as $item): ?>
        <li>
                <a href="<?php echo $item->getHref() ?>">
                    <?php echo $item->getCaption() ?>
                </a>
            </li>
        <?php endforeach ?>
    </ul>
</body>
```

Ce même code, écrit avec TWIG serait :

```text
<body>
    <h1>{{ page_title }}</h1>

    <ul id="navigation">
        {% for item in navigation %}
            <li><a href="{{ item.href }}">{{ item.caption }}</a></li>
        {% endfor %}
    </ul>
</body>
```

## Affichage

* `{{ ma_variable }}` Pour affiche du texte ou un contenu 
* `{# commentaire #}`
* `~ : concatenation`
  * `{{ 'toto' ~ 'titi' }}`

## Variables

TWIG est très puissant lorsqu'il s'agit de manipuler des variables. Pour lui, si c'est un tableau associatif ou un objet avec des propriétés cela est identique dans la syntaxe.

Par exemple si vous avez le tableau $tab\['param1'\] vous écrirez en TWIG

```text
{{tab.param1}}
```

Si vous avez un objet $tab, qui est une instance d'une classe ayant comme propriété $param1, la syntaxe en twig sera :

```text
{{tab.param1}}
```

Si vous avez un objet $tab, qui est une instance d'une classe ayant comme propriété $param1, qui est un tableau associatif avec une clé key1, la syntaxe pourrait être :

```text
{{tab.param1.key1}}
```

La manière dont TWIG inspecte votre code pour trouver la meilleure façon d'interpréter une variable est la suivante :

For convenience's sake foo.bar does the following things on the PHP layer:

* check if foo is an array and bar a valid element;
* if not, and if foo is an object, check that bar is a valid property;
* if not, and if foo is an object, check that bar is a valid method \(even if bar is the constructor - use \_\_construct\(\) instead\);
* if not, and if foo is an object, check that getBar is a valid method;
* if not, and if foo is an object, check that isBar is a valid method;
* if not, and if foo is an object, check that hasBar is a valid method;
* if not, return a null value.

foo\['bar'\] on the other hand only works with PHP arrays:

* check if foo is an array and bar a valid element;
* if not, return a null value.

## Logique

* `{% %}` permet d'utiliser des logiques tels que :
  * `{% if %} {% else %} {%endif %}` : condition
  * `{% for item in items %}{%endfor}` : foreach
  * `{% set foo='foo' %}`  : set des variables

### Tests

Il est possible d'écrire des tests, la syntaxe est très proche de celle de PHP. Attention toutefois, les opérateurs de comparaison ne s'écrive pas de manière identique.

Le && de PHP s'écrit "and" dans TWIG, et le \|\| de PHP s'écrit "or" dans twig.

Il est possible d'avoir des _elseif_ \(autant que nécessaire\) et un bloc _else_ \(1 au maximum\)

```text
{% if condition %}

{% elseif condition %}

{% else %}

{% endif %}
```

### Boucles

Il n'existe que la boucle for dans TWIG \(elle est un peu l'équivalent d'un foreach\).

Le code ci-dessous est une boucle qui varie de 1 à 10.

```text
{% for i in 1..10 %}

{% endfor %}
```

La boucle ci-dessous est une boucle qui parcours une "collection" users \(l'équivalent du foreach\). **Attention la syntaxe est inversée par rapport au PHP**

```text
<ul>
    {% for user in users %}
        <li>{{ user.username }}</li>
    {% endfor %}
</ul>
```

Dans le cadre d'une boucle TWIG propose une variable nommée loop qui permet d'avoir des informations sur la boucle :

| Variable | Description |  |
| :--- | :--- | :--- |
| loop.index | The current iteration of the loop. \(1 indexed\) |  |
| loop.index0 | The current iteration of the loop. \(0 indexed\) |  |
| loop.revindex | The number of iterations from the end of the loop \(1 indexed\) |  |
| loop.revindex0 | The number of iterations from the end of the loop \(0 indexed\) |  |
| loop.first | True if first iteration |  |
|  | loop.last | True if last iteration |
| loop.length | The number of items in the sequence |  |
| loop.parent | The parent context |  |

La boucle ci-dessous intégre un test. Ce qui simplifie l'écriture. Elle intégre également un else dans le cas ou la boucle ne ferait aucune itération. Il est possible d'utiliser le else sans le if et réciproquement.

```text
<ul>
    {% for user in users if user.active %}
        <li>{{ user.username }}</li>
    {% else %}
        <li>No users found</li>
    {% endfor %}
</ul>
```

Le code ci-dessus serait équivalent en PHP au code suivant:

```text
<?php
if (count(users) > 0) {
    foreach ($users as $user) {
        if ($user->active) {
            echo '<li>'.$user->username.'</li>';
        }
    }
}
else {
    echo '<li>No users found</li>';
}
```

## Héritage

Twig permet l'héritage de template via un extends dans les templates enfants :

```text
{% extends 'base.html.twig' %}
```

Dans les templates mère on définit des "block" que l'on vient surcharger dans les templates enfants :

```text
{% block body %}
toto
{% endblock %}
```

On peut également reprendre le block parent via

```text
{{ parent() }}
```

On crée donc des templates mère assez flexibles pour pouvoir en hériter et surcharger les différents blocks

## Exercice 1

* Utiliser l'héritage pour mettre le menu dans un seul fichier mais visible sur les 2 pages.
* Intégrer bootstrap \(CDN\)
* Pour la page /color  afficher le mot de la même couleur dynamiquement \("bleu" en bleu\) \(en CSS\)
* Pour la couleur rouge afficher en plus le Message : "Attention risque de virus" en rouge
* Dans le menu rajouter un lien vers les pages couleurs : red, blue, yellow, pink, violet, salmon en utilisant un foreach
* Mettre en souligné l'url active

## Inclusions

De la même manière que l'on peut hériter d'un tempalte de base, on peut aussi inclure des "morceaux" de template dans une vue. Toujours dans la perspective de ne jamais multiplier un morceau de code identique dans plusieurs fichiers.

## Filtres

Il est possible d'appliquer des filtres sur les variables pour effectuer des transformations : Majuscules, minuscules, dates, ...

```text
{{ variable|upper }}
```

Vous trouverez la liste des filtres sur l'adresse : [Liste des filtres de TWIG](https://twig.symfony.com/doc/2.x/)

Il est possible de cumuler les filtres. Mais il faut faire attention à l'ordre dans lequel on les appliques. Il est aussi possible de créer ses propres filtres.

## Assets

TWIG propose, par l'intermédiaire d'Asset de gérer facilement les URL de vos images, fichiers CSS ou encore javascript.

Pour cela vous aurez besoin d'ajouter le bundle suivant :

```text
composer require symfony/asset
```

Ensuite, vous pouvez écrire dans TWIG

```text
<img src="{{ asset('images/logo.png') }}" alt="Symfony!" />

<link href="{{ asset('css/blog.css') }}" rel="stylesheet" />
```

Par défaut, vos "assets" doivent se trouver dans le répertoire public de Symfony.

**Depuis la version 3.4 et 4, Symfony propose de gérer les assets avec WebPack-Encore, qui est une adaptation de Webpack pour Symfony** Vous trouverez les éléments sur WebPack-Encore ici : [https://symfony.com/doc/current/frontend.html](https://symfony.com/doc/current/frontend.html)

## Exercice 2

* Modifiez le code précédemment écrit pour manipuler des assets directement dans votre projet. Vous intégrer une image de votre choix et Bootstrap en version locale \(supprimer toutes les dépendances aux CDN\).

