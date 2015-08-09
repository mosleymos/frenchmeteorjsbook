{{#template name="commandLine"}}

<h2 id="command-line">Outils en ligne de commande</h2>

#### `meteor help`

Obtenir de l'aide sur l'usage de la commande en ligne de commande `meteor`. 
Lancer `meteor help` va lister les commandes `meteor` les plus communes.
Lancer `meteor help <command>` va imprimer une aide détaillée à propos `meteor <command>`.

#### `meteor create <name>`

Crée un sous  dossier nommé `<name>` et crée une nouvelle application Meteor au sein de celui ci.

#### `meteor run`

Lançe l'application courante à:  [http://localhost:3000](http://localhost:3000)
en usant du serveur local de développement Meteor.

#### `meteor debug`

Lance le projet avec le Node Inspector attaché, ainsi vous pouvez parcourir votre code serveur ligne par ligne. Voir [`meteor debug`](#/full/meteordebug) pour avoir la documentation complète.

#### `meteor deploy <site>`

Mettre en paquet votre application et la déployer sur `<site>`. Meteor fournit un hébergement gratuit si 
vous deployez sur `<your app>.meteor.com` si `<your app>` n'a pas un nom qui est déjà pris par quelqu'un d'autre.

#### `meteor update`

Mettre à jour votre installation Meteor sur la dernière version et
(if `meteor update` was run from an app directory) mettre à jour les packets 
utilisé par une application courante aux dernières versions compatibles avec les autres paquets utilisaés par les autres packets usés par l'application 

#### `meteor add`

Ajoute un paquet (ou de plusieurs packets) à votre projet Meteor. Pour chercher des  
paquets disponibles, utilisez la commande `meteor search`.

#### `meteor remove`

Enlever un paquets precédemment installé dans votre projet Meteor. Pour avoir une liste des paquets  
que votre application utilise actuellement, usez de la commande
`meteor list`.

#### `meteor mongo`

Ouvre  un terminal MongoDB  pour voir et/ou manipuler les collections enregistrées 
dans la base de donnée. Notez que vous devez déjà avoir un serveur qui tourne pour votre  
application courante (dans une autre terminal) pour que `meteor mongo` se connecte à la base de données de l'application. 

#### `meteor reset`

Reset le projet courant à neuf. Enlève toute la donnée locale.

Si vous utilisez `meteor reset` souvent, et que vous avez quelques données initiales que vous ne souhaitez pas effacer.
Penser à utiliser plutôt [`Meteor.startup`](#/basic/Meteor-startup) pour 
recréer la donnée dès le premier lancement du serveur:

```
if (Meteor.isServer) {
  Meteor.startup(function () {
    if (Rooms.find().count() === 0) {
      Rooms.insert({name: "Initial room"});
    }
  });
}
```

{{/template}}
