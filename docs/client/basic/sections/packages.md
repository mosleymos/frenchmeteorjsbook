{{#template name="basicPackages"}}

<h2 id="packages"><span>Paquets</span></h2>

Toutes les fonctionnalités de Meteor sont implémentés en des paquets modulaires. En plus des paquets de base documentés au dessus, il existe plusieurs autres paqutes que vous pouvez ajouter à votre application pour activer une fonctionnalité utile.

Depuis la ligne de commande vous pouvez ajouter des paquets avec `meteor add` et en enlever avec `meteorremove`:

```bash
# add the less package
meteor add less

# remove the less package
meteor remove less
```

Votre application va redemarrer automatiquementa lorsque vous ajoutez ou enlevez un paquet. Les dépendances des paquets de votre application sont suivsi dans `.meteor/packages`, ainsi vos collaborateurs auront une mise à jour automique et le même pack de packages. Dès qu'ils ont le même `.meteor/packages` il auront la même application. 

Vous pouvez voir quels paquets sont utilisés par votre application en utilisant `meteor list` dans le dossier de votre application.

## Chercher des paquets 

Currently the best way to search for packages available from the official
Meteor package server is [Atmosphere](https://atmospherejs.com/), the
community package search website maintained by Percolate Studio. You can
also search for packages directly using the `meteor search` command.

Packages that have a `:` in the name, such as `mquandalle:jade`, are written and
maintained by community members. The prefix before the colon is the name of the
user or organization who created that package. Unprefixed packages are
maintained by Meteor Development Group as part of the Meteor framework.

There are currently over 2000 packages available on Atmosphere. Below is a small
selection of some of the most useful packages.

## accounts-ui

This is a drop-in user interface to Meteor's accounts system. After adding the
package, include it in your templates with `{{dstache}}> loginButtons}}`. The UI
automatically adapts to include controls for any added login services, such as
`accounts-password`, `accounts-facebook`, etc.

[See the docs about accounts-ui above.](#/basic/accounts).

## coffeescript

Utiliser [CoffeeScript](http://coffeescript.org/) dans votre application. Avec ce paquet, n'importe quel fichier avec une extension `.coffee` sera compilée en Javascript avec le build system de Meteor. 

## email

Envoyer des emails depuis votre application. Voir la section [email de l' API
Complete](#/full/email).

<h2 id="jade">mquandalle:jade</h2>

Use the [Jade](http://jade-lang.com/) templating language in your app. After
adding this package, any files with a `.jade` extension will be compiled into
Meteor templates. See the [page on
Atmosphere](https://atmospherejs.com/mquandalle/jade) for details.

## jquery

JQuery makes HTML traversal and manipulation, event handling, and animation
easy with a simple API that works across most browsers.

JQuery is automatically included in every Meteor app since the framework uses it
extensively. See the [JQuery docs](http://jquery.com/) for more details.

## http

Ce paquets permet d'effectuer des requêtes HTTP depuis le client ou le serveur en usant de la même API.
This package allows you to make HTTP requests from the client or server using
the same API. Voir [http docs](#/full/http) pour voir comment l'utiliser.

## less

Ajoutez [LESS](http://lesscss.org/) le preprocessor CSS à votre application pour 
compiler n'importe quel fichier avec une extension `.less`  en CSS standard. Si vous souhaitez user 
`@import` pour inclure d'autre fichiers,  Meteor compilera automatiquement, utilisez l'extension `.import.less`.

## markdown

Inclure code [Markdown](http://daringfireball.net/projects/markdown/syntax) au sein de vos templates. C'est aussi facile que d'utiliser les `{{dstache}}#
markdown}}` helper:

```html
<div class="my-div">
{{dstache}}#markdown}}
# My heading

Some paragraph text
{{dstache}}/markdown}}
</div>
```
Gardez juste votre markdown non indenté puisque les espaces comptent.

## underscore

[Underscore](http://underscorejs.org/) fournit une collection de fonctions utiles pour manipuler les tableaux, objects et fonctions. `underscore` est inclus dans toute application Meteor parce que le framework en luis même s'utilise de manière souple.

## spiderable

Ce paquet permet à votre application du côté serveur d'autoriser les moteurs de recherche crawler et autres bots de voir les contenus de votre application. Si votre préoccupation est le SEO, vous devez ajouter de ce paquet.

{{/template}}
