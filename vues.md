# Vues - TWIG

[La documentation officielle de Symfony sur les vues et TWIG](https://symfony.com/doc/current/templating.html) et [La documentation officielle de TWIG](http://twig.sensiolabs.org/)

* [Présentation](#affichage)
* [Affichage](#affichage)
* [Logique](#logique)
* [Héritage](#héritage)
* [Exos 3](#exos-3)


## Présentation

Twig est un moteur de rendu de template comme Smarty, mais a des rapports très proches avec Symfony, Sensio a contribué énormément à son développement.
Un moteur de template permet de limiter les logiques complexes pour réaliser des templates simples à coder.
La syntaxe commence toujours avec {} des accolades

## Affichage 

* ```{{ ma_variable }}``` Pour affiche du texte ou un contenu 
* ```{# commentaire #}```
* ```~ : concatenation ```
	* ```{{ 'toto' ~ 'titi' }}```

## Logique 

* ```{% %}``` permet d'utiliser des logiques tels que :
	* ```{% if %} {% else %} {%endif %} ``` : condition
	* ```{% for item in items %}{%endfor} ``` : foreach
	* ```{% set foo='foo' %}```  : set des variables

### Tests

### Boucles
	
## Héritage 
Twig permet l'héritage de template via un extends dans les templates enfants :
```
{% extends 'base.html.twig' %}
```

Dans les templates mère on définit des "block" que l'on vient surcharger dans les templates enfants :
```
{% block body %}
toto
{% endblock %}
```

On peut également reprendre le block parent via 
```
{{ parent() }}
```

On crée donc des templates mère assez flexibles pour pouvoir en hériter et surcharger les différents blocks

## Exercice 3 

* Utiliser l'héritage pour mettre le menu dans un seul fichier mais visible sur les 2 pages.
* Intégrer bootstrap (CDN)
* Pour la page /color  afficher le mot de la même couleur dynamiquement ("bleu" en bleu) (en CSS)
* Pour la couleur rouge afficher en plus le Message : "Attention risque de virus" en rouge
* Dans le menu rajouter un lien vers les pages couleurs : red, blue, yellow, pink, violet, salmon en utilisant un foreach
* Mettre en souligné l'url active

## Inclusions

## Filtres

## Assets

