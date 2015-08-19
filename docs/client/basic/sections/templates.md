{{#template name="basicTemplates"}}

<h2 id="templates"><span>Templates</span></h2>

Dans Meteor les vues sont définies en _templates_ . Un template est un bout d'HTML sur lequel on inclue de la donnée dynatimque. Vous pouvez aussi interagir avec vos templates depuis le code javascript pour insérer de la donnée et ecouter des évènements.

<h3 class="api-title" id="defining-templates">Définir des templates en HTML</h3>

Les templates sont des fichiers `.html` qui peuvent être situés n'importe ou dans votre projet Meteor à l'execption des dossier `server`, `public` et `private`.

Chaque fichier `.html` peut contenir plusieurs éléments d'en tête:
Each `.html` file can contain any number of the following top-level elements:
`<head>`, `<body>`, or `<template>`. Code in the `<head>` and `<body>` tags is
appended to that section of the HTML page, and code inside `<template>` tags can
be included using `{{dstache}}> templateName}}`, comme montré dans l'example ci dessous.
Les templates peuvent être inclus plus d'une fois &mdash ; Le but des templates est d'éviter d'ecrire plusieurs fois le même code HTML.

```
<!-- add code to the <head> of the page -->
<head>
  <title>My website!</title>
</head>

<!-- add code to the <body> of the page -->
<body>
  <h1>Hello!</h1>
  {{dstache}}> welcomePage}}
</body>

<!-- define a template called welcomePage -->
<template name="welcomePage">
  <p>Welcome to my website!</p>
</template>
```

La syntaxe `{{dstache}} ... }}` fait partie du langage SpaceBars que Meteor utilise pour ajouter des fonctionnalités au HTML. Comme montré au dessus, SpaceBars vous laisse inclure des templates dans d'autres parties de votre partie. En usant Spacebars, vous pouvez aussi afficher la donnée obtenu par vos _helpers_. Les Helpers sont écrits en Javascript, et peuvent être de simples valeurs ou fonctions.

{{> autoApiBox "Template#helpers"}}

Maintenant voiçi comment vous pouvez définir un helper appellé `name` pour un template nommmé `nametag` (en Javascript):

```
Template.nametag.helpers({
  name: "Ben Bitdiddle"
});
```

Et voiçi le template `nametag` (en HTML):

```
<!-- In an HTML file, display the value of the helper -->
<template name="nametag">
  <p>My name is {{dstache}}name}}.</p>
</template>
```

Spacebars à aussi quelques controles utiles qui peuvent être usés pour rendre vos vues plus dynamiques:

- `{{dstache}}#each data}} ... {{dstache}}/each}}` - Iterate over the items in
`data` and display the HTML inside the block for each one.
- `{{dstache}}#if data}} ... {{dstache}}else}} ... {{dstache}}/if}}` - If `data`
is `true`, display the first block; if it is false, display the second one.
- `{{dstache}}#with data}} ... {{dstache}}/with}}` - Set the data context of
the HTML inside, and display it.

Each nested `#each` or `#with` block has its own _data context_, which is
an object whose properties can be used as helpers inside the block. For
`#with` blocks, the data context is simply the value that appears after
the `#with` and before the `}}` characters. For `#each` blocks, each
element of the given array becomes the data context while the block is
evaluated for that element.

Par example, si le helper `people` a la valeur suivante

```
Template.welcomePage.helpers({
  people: [{name: "Bob"}, {name: "Frank"}, {name: "Alice"}]
});
```

alors vous pouvez afficher tous les noms avec une liste de balises `<p>`:

```html
{{dstache}}#each people}}
  <p>{{dstache}}name}}</p>
{{dstache}}/each}}
```

ou user du template "nametag" au lieu de balises `<p>`:

```html
{{dstache}}#each people}}
  {{dstache}}> nametag}}
{{dstache}}/each}}
```

Souvenez vous que les helpers sont des fonctions aussi bien que de simples valeurs. Par exemple, pour montrer qu'un utilisateur est loggé vous pouvez définir un helper avec une fonction qui retourne la valeur `username`:

```
// in your JS file
Template.profilePage.helpers({
  username: function () {
    return Meteor.user() && Meteor.user().username;
  }
});
```

Maintenat, chaque fois vous utilisez le helper `username`, la fonction au dessous sera appellée pour déterminer le nom d'utilisateur:

```
<!-- in your HTML -->
<template name="profilePage">
  <p>Profile page for {{dstache}}username}}</p>
</template>
```

Les helpers peuvent aussi prendre des arguments. Par exemple, voiçi un helper qui met au pluriel un mot:

```js
Template.post.helpers({
  commentCount: function (numComments) {
    if (numComments === 1) {
      return "1 comment";
    } else {
      return numComments + " comments";
    }
  }
});
```
Passer des arguments en les mettant à l'intérieur des accolades après le nom du helper:

```html
<p>There are {{dstache}}commentCount 3}}.</p>
```
Les helpers au dessus ont été associés à des templates spécifiques, mais vous pouvez rendre un helper disponible dans tous les templates en usant [`Template.registerHelper`](#template_registerhelper).

Vous pourrez trouver une documentation détaillé de Spacebars dans le 
You can find detailed documentation for Spacebars in the [README on GitHub](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md).
Plus tard dans cette documentation, les section sur `Session`, `Tracker`, `Collections` et `Account` parleront un peu plus sur comment ajouter de la donnée dynamique à vos templates.


{{> autoApiBox "Template#events"}}

The event map passed into `Template.myTemplate.events` has event descriptors as
its keys and event handler functions as the values. Event handlers get two
arguments: the event object and the template instance. Event handlers can also
access the data context of the target element in `this`.

Pour attacher des events handlers pour le template suivant

```
<template name="example">
  {{dstache}}#with myHelper}}
    <button class="my-button">My button</button>
    <form>
      <input type="text" name="myInput" />
      <input type="submit" value="Submit Form" />
    </form>
  {{dstache}}/with}}
</template>
```
vous pouvez appeller `Template.example.events` comme dessous:

```
Template.example.events({
  "click .my-button": function (event, template) {
    alert("My button was clicked!");
  },
  "submit form": function (event, template) {
    var inputValue = event.target.myInput.value;
    var helperValue = this;
    alert(inputValue, helperValue);
  }
});
```

The first part of the key (before the first space) is the name of the
event being captured. Pretty much any DOM event is supported. Some common
ones are: `click`, `mousedown`, `mouseup`, `mouseenter`, `mouseleave`,
`keydown`, `keyup`, `keypress`, `focus`, `blur`, and `change`.

The second part of the key (after the first space) is a CSS selector that
indicates which elements to listen to. This can be almost any selector
[supported by JQuery](http://api.jquery.com/category/selectors/).

Whenever the indicated event happens on the selected element, the
corresponding event handler function will be called with the relevant DOM
event object and template instance. See the [Event Maps section](#eventmaps)
for details.
<!-- TODO Update the link to full docs for Event Maps -->

{{> autoApiBox "Template#onRendered"}}

The functions added with this method are called once for every instance of
*Template.myTemplate* when it is inserted into the page for the first time.

These callbacks can be used to integrate external libraries that
aren't familiar with Meteor's automatic view rendering, and need to be
initialized every time HTML is inserted into the page.
You can perform initialization or clean-up on any objects in
[`onCreated`](#template_oncreated) and [`onDestroyed`](#template_ondestroyed)
callbacks.

For example, to use the HighlightJS library to apply code highlighting to
all `<pre>` elements inside the `codeSample` template, you might pass
the following function to `Template.codeSample.onRendered`:

```
Template.codeSample.onRendered(function () {
  hljs.highlightBlock(this.findAll('pre'));
});
```

In the callback function, `this` is bound to a [template
instance](#template_inst) object that is unique to this inclusion of the
template and remains across re-renderings. You can use methods like
[`this.find`](#template_find) and
[`this.findAll`](#template_findAll) to access DOM nodes in the template's
rendered HTML.

<h2 id="template_inst"><span>Template instances</span></h2>

A template instance object represents a single inclusion of a template in the
document.  It can be used to access the HTML elements inside the template and it
can be assigned properties that persist as the template is reactively updated.

Template instance objects can be found in several places:

1. The value of `this` in the `created`, `rendered`,
   and `destroyed` template callbacks
2. The second argument to event handlers
3. As [`Template.instance()`](#template_instance) inside helpers

You can assign additional properties of your choice to the template instance to
keep track of any state relevant to the template. For example, when using the
Google Maps API you could attach the `map` object to the current template
instance to be able to refer to it in helpers and event handlers. Use the
[`onCreated`](#template_onCreated) and [`onDestroyed`](#template_onDestroyed)
callbacks to perform initialization or clean-up.

{{> autoApiBox "Blaze.TemplateInstance#findAll"}}

`template.findAll` returns an array of DOM elements matching `selector`. You can
also use `template.$`, which works exactly like the JQuery `$` function but only
returns elements within `template`.

{{> autoApiBox "Blaze.TemplateInstance#find"}}

<!-- XXX Why is this not findOne? -->

`find` is just like `findAll` but only returns the first element found. Like
`findAll`, `find` only returns elements from inside the template.

{{/template}}
