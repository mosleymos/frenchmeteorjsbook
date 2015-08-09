{{#template name="quickStart"}}
## Démarrage rapide!

Meteor supporte [OS X, Windows, et Linux](https://github.com/meteor/meteor/wiki/Supported-Platforms).

Sur Windows?  [Télécharger l'installeur officiel de Meteor içi](https://install.meteor.com/windows).

Sur OS X or Linux?  Installez la dernière release officielle de Meteor depuis votre terminal:

```bash
$ curl https://install.meteor.com/ | sh
```

L'installeur Windows supporte Windows 7, Windows 8.1, Windows Server
2008, et Windows Server 2012.  L'installateur en ligne de commande supporte Mac OS X
10.7 (Lion) et au-dela, et Linux sur des architectures x86 and x86_64.

Une fois que vous avez intallé Meteor, pour créer un projet:

```bash
$ meteor create myapp
```

Lancez le localement:

```bash
$ cd myapp
$ meteor
# Meteor server running on: http://localhost:3000/
```

Ensuite, ouvrez une nouvelle fenêtre terminal et montrez votre application au monde (sur un serveur gratuit que nous pourvoyons):

```bash
$ meteor deploy myapp.meteor.com
```
{{/template}}
