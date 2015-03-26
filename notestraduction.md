Notes de traduction
=====


# SIMPLE API

## Meteor introduction

Meteor est un environnement ultra simple pour construire des site internet modernes. Ce qui prennait autrefois des semaines prend désormais des heures avec Meteor.


## Principes de Meteor

1. Donnée sur le fil - Meteor n'envoie pas de l'HTML sur le réseau. Le server renvoie de la donnée et le client se charge du rendu.
2. Un seul language - Meteor vous laisse écrire la partie client(front-end) et server(back-end) avec un seul language qui est javascript
3. Base de donnée partout - Vous pouvez utiliser les mêmes méthodes pour accéder vos bases de données depuis le client ou le serveur
4. Compensation de latence(Ndt: Terme qui désigne les technologies qui gère le temps réel) Sur le client, Meteor (prend à l'avance) les données et simule les modèles
5. Réactivité totale - Dans Meteor le temps réel est par défault.
6. Intégrez l'écosystème - Meteor est open source
7. Simplicité est synonyme de productivité - 

# Concepts

## Qu'est ce que Meteor

## Structurer votre application

1. client
2. server
3. public
4. private
    Tous les fichiers à l'interieur d'un dossier private ne sont accessible que depuis le code gérant le serveur et ne peuvent qu'être chargé par l'intermédiaire de l'APi assets.
    Ce dossier peut être utilisé pour des fichiers que vous  souhaitez  
5. client/compatibility 
    Ce dossier comprend des librairies javascript qui repose sur des variables gloabes déclarés dans la portée supérieure.
    Ces fichiers sont executés avant le code coté client.

6. tests
7. lib - Dossier qui est initialisé dans tous les cas dans meteor 

#### Fichier au dehors des dossiers spéciaux

Nb Meteor.isClient et Meteor.isServer sont des globes qui peuvent affecter le code.
Css et Html sont chargé ds le côté client.

Nb Nouveautés client/lib/helpers.js =  

settings.json # Configuration des données passées à meteor

#### Ordre de chargement des fichiers

1. Les templates HTML sont toujours chargés avant tout le reste.
2. Les fichiers commencant par main. sont chargés en dernier.
3. Les fichiers dans n'importe quels dossiers lib sont chargés ensuite.
4. Les fichiers avec les chemins les plus profonds sont chargés ensuite
5. Les fichiers sont chargé dans l'ordre alphabetique du chemin entier.

Ndt: Mettre un exemple

#### Organiser votre projet 

##### Methode 1 Fichier racines

Ndt: Utiliser des graphiques

##### Méthode 2 Dossier à l'intérieur client et serveur

##### Méthode 3 Packages

#### Réactivité

Meteor embrasse le concept de programmation réactive(Ndt:Faire lien vers wikipédia). Cela signifie que vous écrivez votre code de manière impérative et le résultat sera automatiquement recalculé. (Ndt: Concept livereloading hot fix code) et cela qu'importe la donnée sur lequel votre code change.

Cet exemple pris de chat room met en place une subscription basé sur la variable de session currentRoomId. Si la valeur ... change pour n'importe quelle raison, la fonction est automatiquement recalculée remplacant l'ancienne par la nouvelle subscription.



### ANNEXES 
#### Outils utilisés
 Les librairies les plus courantes et à utiliser le plus souvent en javascript sont:
 
 angular.js
 underscore.js
 sugar.js
 jquery.js
 d3.js
 three.js
 maths.js
 require.js

Outil aussi intéressant que fiddlejs - howjs [howjs](http://www.howjs.com)

#### Faire un rendu json par meteor - création d'une forme d'api

Sujet pour rendre du json au lieu de templates

[Stack overflow](http://stackoverflow.com/questions/22915857/meteor-iron-router-without-layout-template-or-json-view/22916375#22916375)
[Stack overflow](http://stackoverflow.com/questions/15601218/json-endpoint-in-meteor)

Snippets to think:

```javascript
// Solution 01
Router.map(function () {
  this.route('api', {
    path: '/api',
    where: 'server',
    action: function () {
      var json = Collection.find().fetch(); // what ever data you want to return
      this.response.setHeader('Content-Type', 'application/json');
      this.response.end(JSON.stringify(json));
  });
});


// Solution 02
this.route('serverRoute', {
  where: 'server',
  path: '/your-route',
  action: function() {
    var data = {this_is:'a json object'}


    this.response.writeHead(200, {'Content-Type': 'application/json'});
    this.response.end(JSON.stringify(data));
  }
});

```

#### Rendu de template en dur

```html

<template name="direct_layout">
    {{> yield}}
</template>

```

```javascript

this.route('rawdata', {
    path: '/raw/:collection',
    layoutTemplate: 'direct_layout',
    template: 'raw'
});

```

##### Approche de développement

1. Create a simple API
2. Create a simple geolocation testimony site
