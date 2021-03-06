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
7. lib - Dossier qui est initialisé dans tous les cas dans meteor, il peut contenir des fois des scripts personnalisées, utilitaires. 

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


## Collections

Meteor stocke les données au sein de collections. Premièrement créez une collection avec Mongo.Collection.

SYNOPSIS

#### new Mongo.Collection(name, [options])

name - (STRING) Nom de la collection

[options]
    - connection (OBJET) La connexion serveur qui va gérer la collections. Utilise la connection par défaut si elle est non spécifié. Passez la valeur de retour de DDP.connect pour spécifier un serveur différent. Passez la valeur null pour spécifier l'absence de connection.
    - idGeneration (STRING) La méthode pour générer des champs _id de nouveaux documents dans la collection. Les valeurs possibles sont 
        1. 'STRING': chaînes aléatoires
        2. 'MONGO': valeurs aléatoire Mongo.ObjectID
    - Transform
        Une fonction optionnelle de transformation. Les documents seront passés au travers de cette fonction avant d'être retourné par un fetch ou un findOne et aussi avant d'être passé par les callbacks observe, map, forEach, allow et deny. Les transformations ne sont pas appliqués pour les callbacks observeChanges ou aux curseurs retournés des fonction de publication.

Appeller une fonction est analogue à déclarer un model dans un ORM (Object Relationnal Mapper) classique centré sur un framework. Il met en place une collection (un espace dédié aux enregistrement, ou documents) qui eput être utilisé pour stocker un type particulier d'information, tels que les utilisateurs, posts, scores, taches, ou ce qui importe à votre application.
Chaque document est un objet EJSON. Il inclue une _id propriété dont la valeur unique est dans la collection auquel Meteor vous enverra dès que le document sera crée.
```
// collection.
Chatrooms = new Mongo.Collection("chatrooms");
Messages = new Mongo.Collection("messages");

```
 Une fonction qui retourne un objet avec des methodes pour inserer des documents dans la collection, mettre à jour leurs propriétes, et les enlever et de trouver les documents au sein de al collection qui correspondent à n'importe quel critère. La manière dont fonctionne ces methodes travaille est compatible avec la base de donnée Mongo. La même base fonctionne à la fois sur client et serveur.

```
// return array of my messages
var myMessages = Messages.find({userId: Session.get('myUserId')}).fetch();

// create a new message
Messages.insert({text: "Hello, world!"});

// mark my first message as "important"
Messages.update(myMessages[0]._id, {$set: {important: true}});
```

#### collection.find([selector], [options])

Trouve les documents dans une collection qui correspondent au sélécteur

    - selector (Selecteur Mongo, ObjectId ou Chaine)
        Une requête qui décrit les documents à trouver

    - Options
        sort (Mongo Sort Specifier)
            Trie 
        skip (Nombre)
            Nombre de résultat à skipper depuis le début
        limit (Nombre)
            Nombre maximal de resultat à renvoyer
        fields (Mongo Field Specifier)
            Dictionnaire des champs à retourner ou exclure
        reactive (Boolean)
            Uniquement sur le client - Par défaut true ; mettre à false si l'on veut désactiver la réactivité
        transform (Fonction)
            Verrouille transformation sur la collection

find retourne un curseur. Le curseur ne donne pas un accès immédiat à la base de donnée ou retourne des documents. Les curseurs fourniessent une fonction fetch qui retourne tous les documents qui remplissent la condition. Map et forEach itèrent sur tous les documents correspondants. Observe et ObserveChanges lance les callbacks lorsqu'un set de documents changent.
Les curseurs sont des sources de données réactives. Sur le client, la première fois que vous récupérez un documents avec curseur avec fetch, map ou forEach, Meteor va enregistrer une dépendance avec la donnée.
Tout changement sur la collection va changer le curseur qui va relancer une computation. Pour arrêter ce comportement, passez { reactive: false } comme une option pour find.

Noez que lorsque fields sont spécifiés, seulement les changements sur les chan 
L'utilisation avec attentions de fields permet une réactivité au grain pour les computation qui ne dépendent par d'un document entier.

#### collection.allow(options)

Autorise les utilisateurs a écrire directement dans la collection depuis le code coté client, sujet à des limitations que vous définissez

    Options
        insert, update, remove Function
        Function qui verifie une proposition de modification dans la base de donnée et renvoie vrai si elle est autorisée.

Dans les application nouvellement crée, Meteor autorise la pulpart des appels à insert, update et remove depuis n'importe quel code côté client et/ou serveur. La raison est que lorsque l'application est crée par meteor create, le paquet insecure est inclus par défaut pour simplifier le développement. Clairement, si n'importe quel utilisateur pouvait changer la base de donnée lorsqu'il en a envie, cela serait mauvais pour la sécurité, ainsi il est important d'enlever le paquet insecure et de mettre quelques règles de sécurité. 

```

meteor remove insecure

```

Une fois le paquet meteor insecure enlevé, usez des methodes allow et deny pour controller qui peut opérer des des opérations dans la base de donnée. Par défatu toutes les opérations du client sont refusées, soit nous aurons besoin d'ajouter des règles allow. Gardez en tête quele code serveur et le code à l'intérieur des méthodes ne sont pas affecté par allow et deny - ces règles ne s'applique que lorsque insert, update et remove sont appellé depuis un code client non verifié , ou trusté  -- auquel on a pas confiance.

Par example nous aurions des utilisateurs qui souhaiteraient créer de nouveaux posts si les champs createdBy correspondent au ID de l'utilisateur courant, ainsi les utilisateurs ne peuvent ne pas  s'impersonner ???? - > a modifier

```
// In a file loaded on the server (ignored on the client)
Posts.allow({
  insert: function (userId, post) {
    // can only create posts where you are the author
    return post.createdBy === userId;
  },
  remove: function (userId, post) {
    // can only delete your own posts
    return post.createdBy === userId;
  }
  // since there is no update field, all updates
  // are automatically denied
});

```

La methode allow accepte trois possible callbacks: insert, remove et update. Le premier argument des trois callbacks ets le _id de l'utilisateur couramment connecté, et les arguments restants sont les suivants.

1. insert(userId, document)
    document est le document qui va être inseré au sein de la base de donnée. Retournez true si l'insert est autorisé, false autrement.
2. update(userId, document, fielNames, modifier)
    document est le document qui va être modifié. fieldNames est un tableau de haut niveau avec les fields qui sont affécté par ce changement. modifier est le Mongo Modifier qui est passé en second argument de collection.update. Il peut être difficile d'achever une validation correcte en usant de ce callback, il est ainsi recommandé d'user de méthodes. Retourenz true si l'update est autorisée false autrement.
3. remove(userId, document)
    document est le document qui va être enlevé de la base de donnée, retourne true si le document doit être enlevé, false autrement.


#### collection.deny(options)

Verrouille les règles allow

Options
    insert, update, remove Function

    Fonctions qui regarde la modification proposé pour la base de donnée et retourne true, si elle est refusé même si un allow en dit autrement.

La méthode deny vous laisse de manière sélective verrouiller vos règles allow. Cependant seulement un seul des allow callbacks doit retourner true pour autoriser une modification, tous vos deny callback doivent renvoyer false.

Par example, si nous voulons verrouiller la partie de notre règle allow au dessus pour exclure l'envoi de certains titres de postes:

```

// In a file loaded on the server (ignored on the client)
Posts.deny({
  insert: function (userId, post) {
    // Don't allow posts with a certain title
    return post.title === "First!";
  }
});

```

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

### Informations supplementaires sur tracker

Tracker.autorun(runFunc, [options])    ---- Client

Lance une fonction et la relance plus tard lorsque les dépendences changent.
Retourne un objet de type computation qui peut être utilisé pour arrêter la computation ou observer le changement

Arguments 

   runFunc Function
   La fonction à faire tourner, elle recoit un argument , l'objet de type computation sur lequel sera fait un retour

   onError Function
   Optionnel. La fonction a relancer dès lors qu'une erreur apparaît dans la computation. Le seul argument recu est l'erreur envoyée. Les erreurs par défauts sont loggés sur la console.

Tracker.autorun vous autorise à lancer une fonction qui dépend d'une source de donnée reactive, de tel manière que lorsqu'une donée est changée 

Par exemple vous pouvez surveiller un curseur (qui est une source de donnée réactive) et agréger au sein d'une variable de session.

```javascript

```
### BLAZE

Blaze*.render(templateOrView, parentNode, [nextNode], [parentView]) --------------Client

Rend un template ou une vue et l'insère au niveau du DOM, retournant une vue rendue, qui peut être placé à Blaze.remove

Arguments
    TemplateOrView Blaze.Template ou Blaze.view
        Le template ou vue à générer. Si c'est un template, un objet vue est construit. Si c'est une vue, il doit être non

    ParentNode DOM Node
        Le node qui va être parent du template rendu. Il doit être un node Elément.

    NextNode DOM Node
        Optionnel, s'il est donné, il doit être enfant du parentNode; le template doit être inséré avant le Node. S'il n'est pas fourni, il sera inséré comme dernier noeud enfant du parentNode.



### Assets
Assets autorise le code dans la partie serveur d'une application Meteor d'accéder aux assets statiques qui sont localisés au sein du dossier private de l'arbre de l'application. Les assets ne sont pas pris en compte comme fichiers sources et son copiés directement au dans le package de l'application.

Assets.getText(assetPath, [asyncCallback]) ------Partie serveur
    Récupère les contenus statiques du serveur sous un encodage UTF8 
        Arguments

            assetPath String
                Le chemin de l'asset relatif au dossier private de l'application
            asyncCallback Fonction 
                Callback optionnel qui est appellé de manière asynchrone avec une erreur ou un résultat dès lors que l'éxecution de la fonction est complète. Si le callback n'est pas fourni, la fonction est lancé en synchrone.

Assets.getBinary(assetPath, [asyncCallback]) -----------Partie Serveur
    Récupère les contenu static du server comme des EJSON BINARY
        Arguments
        
            assetPath String
                Le chemin de l'asset, est relatif du dossier private de l'application
            asyncCallback Fonction
                Callback optionnel qui est appellé de manière asynchrone avec une erreur ou un résultat dès lors que l'éxecution de la fonction est complète. Si le callback n'est pas fourni, la fonction est lancé en synchrone.

Les assets statiques du serveur sont inclus en les places dans le dossier private. Par exemple si une application a un sous dossier private qui contient un dossier nested avec un fichier appellé data.txt à l'intérieur alors le code serveur peut lire data.txt en lançant:

```

var data = Assets.getText('nested/data.txt')

```


### Templates
Lorsque vous écrivez un template tel que 
```
<template name='foo'></template>`
```
Meteor génère un objet template nommé Template.foo

Le même template peut être mis plusieurs fois sur une page et ses occurence s sont nommées des instances. Les instances de templates on une durée de vie  lorsqu'elle sont crées`

Template.myTemplate.onRendered --------------Client

Enregistre une fonoction qui est appellé lorsqu'une instance du template est inserée dans le DOM.
    Arguments
        CallBack - Function
            fonction qui est ajouté comme callback


Les callbacks sont ajoutés avec cette méthode sont appellés une fois lorsqu'une instace de Template.mytemplate sont rendus en noeuds du DOM et mis dans le domcument pour la première fois.

Dans le corps du callback, this est une instance de l'objet template qui est unique.
Utilisez les callbacks onCreated et onDestroyed pour effectuer une initialisation ou un nettoyage de l'objet.

Parce que votre template a été déjà rendu, vous pouvez utiliser les fonction this.findAll qui regardent au travers de noeuds du DOM.

Cela peut être une bonne place pour appliquer n'importe quelle manipulation du DOM que vous souhaitez après que le template soit rendu une première fois.

```

<template name="myPictures">
  <div class="container">
    
  </div>
</template>



Template.myPictures.onRendered(function () {
  // Use the Packery jQuery plugin
  this.$('.container').packery({
    itemSelector: '.item',
    gutter: 10
  }*E);
});

```

Template.myTemplate.onCreated---------------Client

Enregistre une fonction a appeller lorsque une instance de ce temp

Arguments
  callback function
    Une fonction a être ajouté comme callback

Les callbacks sont ajoutés avec cette methode avant que la logique de votre template est évalué la première fois. A l'intérieur su callback, this correspond à l'in
Les propriétés que vous initialisez sur l'objet seront visibles depuis les callbacks ajoutés avec onRendered et onDestroyed et des gestionnaires d'évènements. 

Ces callbacks se lancent une seule fois et sont le ... Gérer l'évènement create est une manière utile de mettre les valeurs au sein de l'instance du template et de lire depuis les template helpers en usant de Template.instance().

```

Template.myPictures.onCreated(function () {
  // set up local reactive variables
  this.highlightedPicture = new ReactiveVar(null);

  // register this template within some central store
  GalleryTemplates.push(this);
});

```

Ndt
Possibilité de faire template.subscribe



### Packages




### EJSON

EJSON est une extension du JSON qui supporte un plus grand nombre de types. EJSON supporte tous les types JSON-types safes tles que 
1. Date (Javascript Date)
2. Binary (Javascript Uint_Array ou les résultat de EJSON.newBinary)
3. Types définis par l'utilisateur (voir Ejson addType) Par example Mongo.ObjetId est implémenté de cette manière

Toutes les sérialisations EJSON sont aussi valides en JSON. Par example un objet avec une date et un buffer binaire peut être sérialisé en EJSON comme 

```
{
  "d": {"$date": 1358205756553},
  "b": {"$binary": "c3VyZS4="}
}
```

### Architecture METEOR - METEOR MANUAL

[Meteor Manual](http://manual.meteor.com/)

Bienvenue dans le manuel Meteor ! Içi nous allons vous enseigner comment utiliser le potentiel de meteor ainsi qu'expliquer les décision et l'ingénierie derrière chaque fonctionnalité. La première section concernera les Deps, une petite mais puissante librairie qui permet la programmation réactive de manière transparente. Dans le second chapitre nous aborderons Blaze le systeme de template réactif de Meteor. Les derniers chapitres concerneront le DDP (le protocole de données distribuées) et Unibuild (un outil de gestion des paquets et de leur construction).

#### Deps
##### Aperçu

Meteor Deps est une librairie incroyablement petite (1 k) mais incroyablement puissante pour la programmation réactive transparente en javascript.

##### Programmation Réactive transparente
Un problème basique survient lorsque l'on écrit des application, surtout dès lors que l'on essaie de surveiller le changement de quelques valeurs tel qu'une donnée au sein d'une base de données, l'objet couramment selectionné dans une table, la largeur de la fenêtre, ou le temps actuel et mettre à jour lorsque n'importe quelle de ces valeurs change.

Il y a plusieurs stratégies couramment usées pour exprimer ces idées:

1. Poll and diff - De manière périodique (toutes les secondes par exemple), attraper la valeur courante de l'objet et voir si elle est changée et ensuite procéder à la mise à jour.
2. Events - L'objet changé emet un évènement lorsqu'il change. Une autre part du programme (souvent le controller) se débrouille pour écouter l'évènement, obtient la valeur courante et procède à la mise à jour lorsque l'évènement est lancé.
3. Bindings - Les valeurs sont representés par des objets qui implémentent des interfaces comme bindableValue. Alors une methode bind est utilisé pour relier deux BindableValues ensemble de telle sorte que lorsqu'une valeur change, l'autre est mise à jour de manière automatique.



3. Building a User Interface with Transparent Reactive Programming
Une librairie que vous pouvez utiliser est ReactiveDict qui fournit un simple objet de type dictionnaire qui rend les données totalement réactives

```javascript
var forecasts = new ReactiveDict;
forecasts.set("san-francisco", "cloudy");
forecasts.get("san-francisco");
// "cloudy"

```

Maintenant mettons notre météo courante pour San Francisco sur l'écran et rendons les mises à jour réactives dès lors que la météo change. Le code est étonnament court.

```
$('body').html("The weather here is <span class='forecast'></span>!");
Deps.autorun(function () {
  $('.forecast').text(forecasts.get('san-francisco'));
});
// Page now says "The weather here is cloudy!"

forecasts.set("san-francisco", "foggy");
// Page updates to say "The weather here is foggy!"

```

Dans une application réelle vous n'aurez jamais à écrire autant de code si vous utilisez une librairie tel que Meteor Blaze qui vous laise utiliser son systeme de templating tels que:
```
<template name="weather">
  The weather here is {{forecast}}!
</template>

// In app.js
Template.weather.forecast = function () {
  return forecasts.get("san-francisco");
};

```
Blaze convertit le template et met à jour autrun sur tout les éléments nécessaires. Mais fondatalement, ce que Blaze effectue est vraiment simple, il laisse simplement exprimer l'interface de votre application en HTML au lieu de code javascript.

Le pouveoir des deps provient du fait que vous pouvez utilisez n'importe quel code javascript pour calculer ce qui va apparaître au sein de l'écran, et cela sera automatiquement réactif. Voici un exemple plus complexe.

```
var forecasts = new ReactiveDict;
forecasts.set("Chicago", "cloudy");
forecasts.set("Tokyo", "sunny");

var settings = new ReactiveDict;
settings.set("city", "Chicago");

$('body').html("The weather in <span class='city'></span> is <span class='weather'></span>.");
Deps.autorun(function () {
  console.log("Updating");
  var currentCity = settings.get('city');
  $('.city').text(currentCity);
  $('.weather').text(forecasts.get(currentCity).toUpperCase());
});
// Prints "Updating"
// Page now says "The weather in Chicago is CLOUDY."
```

Dans cet exemple une seule variable réactive ('city' dans settings) est usé pour déterminer quel autre variable réactive sera surveillé ( forecast pour cette ville), et aussi les prévision qui change de case lorsque -> a améliorer

Lorsque l'on change dans l'immédiat n'importe quelle valeur réactive, la page de met à jour automatiquement.

```
settings.set("city", "Tokyo");
// Prints "Updating"
// Page updates to "The weather in Tokyo is SUNNY."

forecasts.set("Tokyo", "wet");
// Prints "Updating"
// Page updates to "The weather in Tokyo is WET."

```

Mais lorque la valeur reactive n'est pas nécessaire pour le rendu de la page, rien ne se passe , parce que Deps sait que la valeur n'a pas été utilisé.

```

forecasts.set("Chicago", "warm");
// Does *not* print "Updating"
// No work is done

```

Comme n'importe quelle application écrite avec Deps le code se décompose naturellement en trois parties:

1. La source de données réactive, dans ce cas la librairie ReactiveDict. Une application réelle utilise une variété de librairies qui fournissent des valeures réactive, tel qu'une base de donné&es 
2. Un traitement de la donnée réactive, dans ce cas un simple autorun qui use de jquery pour mettre à jour le DOM, mais dans une application réele
3. Une logique d'application, dans ce cas le code actuel qui récupère la prévision appropriée

L'idée cle des deps est que le code d'application n'a pas à être réactif du tout. Ceci est la programmation transparente réactive. Le code d'application use usete des application public des sources de données réactives(currentCIty.get), ou une requete normale MongoDb usant de minimongo par example). Le code de l'application n'a jamais fait appel à une des Deps API. Seulement les créateurs de nouvelles données de sources réactive.

### Comment Deps Works ?



### ANNEXES 


#### Clustering en meteorjs

Article sur le sujet exploré par le site meteorhacks
[Meteor Hacks](https://meteorhacks.com/cluster-a-different-kind-of-load-balancer-for-meteor) 

Le paquet est meteorhacks:cluster


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

Sujet pour rendre du json au lieu de templates - sujet pris sur stack overflow

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

ongoworks:security - gestion securité sous meteor


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
meteorhacks:kadira-debugs


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
    MeteorCasts - Site




##### Approche de développement

1. Create a simple API
2. Create a simple geolocation testimony site



