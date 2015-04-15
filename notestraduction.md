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


## Elements de base du framework meteorjs

1. Templates - Créez des vues qui se mettent à jour automatiquement lorsque la donnée change.
2. Session - Stockez de manière temporaire les données pour l'interface utilisateur
3. Tracker - Relancer une fonction lorsque la donnée change.
4. Collection - Stocker de la donnée persistente
5. Comptes - Laissez vos utilisateurs s'enregistrer, se connecter avec des mots de passes ou Facebook, Google, Github ...
6. Methodes - Appellez a partir du client des methodes sur le serveur
7. Publier/Souscrire - Synchroniser une partie de votre donnée dans le client
8. Environnement - COntroller quand et où votre code tourne.
9. Paquets - Choisissez parmi les milliers de paquets de la communauté.

# Concepts

## Qu'est ce que Meteor

Meteor est un environnement ultra-simple pour la construction des sites Web modernes. Ce qui prenait auparavant des semaines,  même avec les meilleurs outils, prend maintenant quelques heures avec Meteor.

Le web a été conçu pour fonctionner de la même manière que les mainframes des années 70. Le serveur d'application effectuait un rendu et l'envoyait sur le réseau à un terminal. Chaque fois que l'utilisateur a fait quelque chose, ce serveur générait un tout nouveau écran. Ce modèle a servi le Web pendant près d'une décennie. Il a donné naissance à LAMP, Rails, Django, PHP.

Cependant il est désormais possible créer des applications en JavaScript qui se exécutent sur le client avec les mêmes équipes, les mêmes gros budgets et les mêmes horaires.  Ces applications ont des interfaces locales. On n'a pas besoin de recharger les pages. Ces applications javascript sont réactives:  les changements effectués de ne importe quel poste apparaissent immédiatement sur l'écran des autres.

Ces applications sont codés à la "dure". Meteor les a simplifiés et rendu plus ludiques. Vous pouvez construire une application complète en un week-end ou au sein d'un hackathon avec suffisamment de café. Vous n'avez plus besoin de ressources du serveur, ou de déployer des paramètres de l'API dans le cloud, ou de gérer une base de données, ou d'utiliser une couche ORM, ou de gérer dans les deux sens les invalidations de données entre Javascript et Ruby, ou la diffusion de données entre des clients.

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



## Sessions

La Session fournit un objet global dans le client que vous pouvez utiliser pour stocker un ensemble arbitraire de données clé-valeur. Utilisez-la session pour stocker des informations comme l'élément actuellement sélectionné dans une liste.

La particularité de la  session est que celle ci est réactive. Si vous appelez Session.get ("maCle") dans un Template helper ou à l'intérieur de Tracker.autorun, la partie valeur de la clé sera de nouveau rendu de manière automatique chaque fois que Session.set ("maCle", nouvelle valeur) est appelée.


### Session.set 

Défini une variable au sein de la session.


### Session.get

Obtenir la valeur d'une variable de session.

### Exemple d'utilisation des sessions

Exemple:

```javascript

<!-- In your template -->
<template name="main">
  <p>We've always been at war with {{theEnemy}}.</p>
</template>

// In your JavaScript
Template.main.helpers({
  theEnemy: function () {
    return Session.get("enemy");
  }
});

Session.set("enemy", "Eastasia");
// Page will say "We've always been at war with Eastasia"

Session.set("enemy", "Eurasia");
// Page will change to say "We've always been at war with Eurasia"

```

Utiliser la session nous donne un premier goût de la réactivité, l'idée que la vue se met à jour automatiquement si nécessaire, sans avoir à appeler une fonction.  Dans la section suivante, nous allons apprendre à utiliser Tracker, la bibliothèque qui rend cela possible dans Meteor.


### TRACKER

Meteor a un système de gestion des dépendances simple qui permet de relancer et mettre à jour de manière automatique les templates, les variables de session, et les bases de donnnées dès lors que les données changent.

A l'inverse des autres systèmes, vous n'avez pas à définir ces dépendances, "ça fonctionne", le mécanisme est simple et efficace. Une fois que vous avez initialisé une computation avec Tracker.autorun chaque fois que vous appelez une fonction de Meteor qui renvoie des données, Tracker enregistre automatiquement les données qui ont été accessibles. Plus tard, lorsque une modification des données est effectuée, le calcul est relancé automatiquement. C'est ainsi un modèle sait comment re-rendre chaque fois ses fonctions d'aide ont de nouvelles données à renvoyer.


Tracker.autorun vous permet d'exécuter une fonction qui dépend de sources de données réactives. Chaque fois qu'une de ces sources de données sont mises à jour avec de nouvelles données, la fonction pourra être relancée.

Par exemple, vous pouvez surveiller une variable de session et de fixer une autre:

```javascript
Tracker.autorun(function () {
  var celsius = Session.get("celsius");
  Session.set("fahrenheit", celsius * 9/5 + 32);
});

```

Ou vous pouvez attendre qu'une variable de session ait une certaine valeur, et effectuer un calcul. Si vous souhaiter l'arrêt d'un tracker vous devriez appeller la methode stop() sur la computation :

```javascript

// Initialize a session variable called "counter" to 0
Session.set("counter", 0);

// The autorun function runs but does not alert (counter: 0)
Tracker.autorun(function (computation) {
  if (Session.get("counter") === 2) {
    computation.stop();
    alert("counter reached two");
  }
});

// The autorun function runs but does not alert (counter: 1)
Session.set("counter", Session.get("counter") + 1);

// The autorun function runs and alerts "counter reached two"
Session.set("counter", Session.get("counter") + 1);

// The autorun function no longer runs (counter: 3)
Session.set("counter", Session.get("counter") + 1);

```

### Architecture METEOR - METEOR MANUAL

[Meteor Manual](http://manual.meteor.com/)



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
#### Paquets les plus utilisés au sein de la communauté meteor

Liste des paquets les plus utilisés au sein de meteorjs

sacha:spin - Spinner

jquery - manipulation du DOM 

underscore - fonctions javascriptes non natives

twbs:bootstrap - official bootstrap css

tap:i18  - internationalisation

mail - envoi de mail

accounts-ui - User log_in log_out helpers

accounts-password - User accounts

iron:router - routing in meteor

pauldowman:dotenv - Gestion des variables d'environnement

spiderable - Référencement  SEO

http - Faire des requete http

markdown - Html simplifié

Ensemble des paquets aldeed:

aldeed:simple-schema
aldeed:template-extension
aldeed:autoform

Modals

anti:modals

Outils développement

msavin:mongol

Pour faire un peu de 3D
acemtp:x3dom 


Paquets utiles 

accounts-password 
useraccounts:core 
reactive-var 	
reactive-dict 		
iron:router 	
zimme:iron-router-active 
zimme:iron-router-auth 
manuelschoebel:ms-seo 
dburles:collection-helpers 
matb33:collection-hooks 
reywood:publish-composite 
ongoworks:security
alanning:roles 
aldeed:autoform
aldeed:collection2
aldeed:simple-schema
momentjs:moment 
matteodem:easy-search 
matteodem:server-session 
meteorhacks:kadira
meteorhacks:aggregate 
meteorhacks:fast-render 
meteorhacks:subs-manager
meteorhacks:unblock 
raix:handlebar-helpers 
yogiben:helpers 
zimme:collection-softremovable 
zimme:collection-timestampable 
u2622:persistent-session 
tmeasday:publish-counts 
percolatestudio:synced-cron 
dburles:factory 
anti:fake 
The rabbit hole goes deeper...

Wow, you must really be committed if you got this far. Ok, so you want my super secret lists?
Service Providers

When you go to deploy your app online there are a huge amount of service providers available to a developer. Below are a few that specifically serve the Meteor community (and do a great job) so I decided to give them a shout.

    Kadira - Performance Tracking
    Modulus - Hosting (Use code 'Metpodcast' to get a $25 credit)
    Compose - Mongo Database Hosting with Oplog

Blogs, Vlogs, News & more (no order)

Come drink the Meteor cool-aid with me... look we won't be alone.

    Crater.io - News Aggregate
    Meteor Weekly - News Aggregate
    Meteor's Official Blog - Blog
    Josh Owens - Blog
    Discover Meteor - Blog
    The Meteor Chef - Blog
    Differential - Blog
    Gentlenode - Blog
    MeteorHacks - Blog
    Meteor Tips - Blog
    PEM - Blog
    Manuel Schoebel - Blog
    Practical Meteor - Blog
    Lukasz Kups - Blog
    David Burles - Blog
    David Weldon - Blog
    The Meteor Podcast - Podcast
    Meteor Devshops - YouTube
    Josh Owens - YouTube
    George McKnight - YouTube
    Arunoda Susiripala - YouTube
    David Turnball - YouTube
    Sasi Kanth - YouTube
    Vianney Lecroart - Medium
    Space Camp - Medium
    Dominus - Medium
    Arunoda Susiripala - Medium
    Sacha Greif - Medium
    Paul van Zyl - Medium




##### Approche de développement

1. Create a simple API
2. Create a simple geolocation testimony site



### Templates
Lorsque vous écrivez un template tel que 
```
<template name='foo'></template>`
```
Meteor génère un objet template nommé Template.foo

Le même template peut être mis plusieurs fois sur une page et ses occurence s sont nommées des instances. Les instances de templates on une durée de vie  lorsqu'elle sont crées`
