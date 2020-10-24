# Alpine.js

![npm bundle size](https://img.shields.io/bundlephobia/minzip/alpinejs)
![npm version](https://img.shields.io/npm/v/alpinejs)
[![Chat](https://img.shields.io/badge/chat-on%20discord-7289da.svg?sanitize=true)](https://alpinejs.codewithhugo.com/chat/)

Alpine.js offre les fonctionnalités de réactivité et déclaration des frameworks comme Vue et React à moindre frais.

Vous pouvez garder votre DOM, et incorporer ses fonctionnalité aux besoins.

Imaginer [Tailwind](https://tailwindcss.com/) mais pour le JavaScript.

> Note: La syntax est presque entièrement emprunter de [Vue](https://vuejs.org/) (et par extension [Angular](https://angularjs.org/)). Je suis reconnaissant de ce qu'ils apportent au web.

## Installation

**CDN:** Ajouter le script suivant a la fin de la section `<head>`.
```html
<script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.min.js" defer></script>
```

C'est tout. Alpine.js s'occupe de s'initialiser.

Pour votre environnement de production, il est recommandé de spécifier un numéro de version dans le lien précédent afin de ne pas subir des indésirables de futures versions.

Par exemple, utiliser `2.6.0` (dernière):
```html
<script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.6.0/dist/alpine.min.js" defer></script>
```

**NPM:** Installer le script via NPM.
```js
npm i alpinejs
```

Inclure Alpine.js dans votre script.
```js
import 'alpinejs'
```

**Pour le support IE11** Utiliser les script suivant a la place.
```html
<script type="module" src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.min.js"></script>
<script nomodule src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine-ie11.min.js" defer></script>
```

Le modèle ci-dessus est le [module/nomodule pattern](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/) qui se charge de générer un paquet qui sera automatiquement charger par les navigateurs modernes, le paquet IE11 sera charger automatiquement par IE11 et autres navigateurs moins récents.

## Utilisation

*Liste déroulante/Modale*
```html
<div x-data="{ open: false }">
    <button @click="open = true">Ouvrir liste déroulante</button>

    <ul
        x-show="open"
        @click.away="open = false"
    >
        Corps liste déroulante
    </ul>
</div>
```

*Onglets*
```html
<div x-data="{ tab: 'foo' }">
    <button :class="{ 'active': tab === 'foo' }" @click="tab = 'foo'">Foo</button>
    <button :class="{ 'active': tab === 'bar' }" @click="tab = 'bar'">Bar</button>

    <div x-show="tab === 'foo'">Tab Foo</div>
    <div x-show="tab === 'bar'">Tab Bar</div>
</div>
```

Il est également possible de l'utiliser pour des fonctions moins tricials:
*Pre-charger une liste déroulante au survol*
```html
<div x-data="{ open: false }">
    <button
        @mouseenter.once="
            fetch('/dropdown-partial.html')
                .then(response => response.text())
                .then(html => { $refs.dropdown.innerHTML = html })
        "
        @click="open = true"
    >Montrer liste déroulante</button>

    <div x-ref="dropdown" x-show="open" @click.away="open = false">
        Chargement...
    </div>
</div>
```

## Apprendre

14 directives sont à disposition:

| Directive | Description |
| --- | --- |
| [`x-data`](#x-data) | Déclare une nouvelle portée(scope) pour un composant. |
| [`x-init`](#x-init) | Execute une expression à l'initialisation d'un composant. |
| [`x-show`](#x-show) | Bascule `display: none;` sur un élément en fonction du résultat de l'expression passée en paramètre (true ou false). |
| [`x-bind`](#x-bind) | Assigne la valeur d'un attribut avec le résultat d'une expression JS. |
| [`x-on`](#x-on) | Attache un écouteur d'événement à l'élément. L'expression sera exécuter lorsque l'événement est émi. |
| [`x-model`](#x-model) | Ajoute une "contrainte bidirectionnelle" à un élément. Cela synchronise les élément de formulaire avec les donéées du composants. |
| [`x-text`](#x-text) | Similaire à `x-bind`, mais tient à jour l'`innerText` de l'élément. |
| [`x-html`](#x-html) | Similaire à `x-bind`, mais tient à jour l'`innerHTML` de l'élement. |
| [`x-ref`](#x-ref) | Utile pour récupérer l'élément DOM brut ducomposant. |
| [`x-if`](#x-if) | Retire un élément du DOM en fonction du retour de l'expression (true ou false). Doit être utilisée sur un tag  `<template>`. |
| [`x-for`](#x-for) | Créer un nouveau noeud DOM pour chaque membre du tableau passée en paramètre. Doit être utilisée sur un tag  `<template>`. |
| [`x-transition`](#x-transition) | Directives permettant d'appliquer des classes en fonction de l'état de transition de l'élément. |
| [`x-spread`](#x-spread) | Permet de lier un object de directies Alpine.js à un élément, pour une meilleure réutilisabilité. |
| [`x-cloak`](#x-cloak) | Retiré une fois Alpine.js initialiser. Pratique pour cacher des élément de pré-initialisation du DOM. |

Plus 6 propriétés magiques:

| Propriété magique | Description |
| --- | --- |
| [`$el`](#el) |  Retourne l'élément DOM du composant principale(root). |
| [`$refs`](#refs) | Retourne l'élément DOM de l'élément marquer avec l'attribut `x-ref` dans le composant. |
| [`$event`](#event) | Retourve l'objet natif "Event" a l'intéeiur d'un écouteur d'événement.  |
| [`$dispatch`](#dispatch) | Créer un `CustomEvent` et déclenché le, en interne, via `.dispatchEvent()`. |
| [`$nextTick`](#nexttick) | Execute une expression APRÉS que Alpine.js est fait ses mises à jour réactives du DOM. |
| [`$watch`](#watch) | Déclenche un rappel(callback) quand vous "observer" les changements d'une propriété du composant. |


## Sponsors

<img width="33%" src="https://refactoringui.nyc3.cdn.digitaloceanspaces.com/tailwind-logo.svg" alt="Tailwind CSS">

**Votre logo içi? [DM on Twitter](https://twitter.com/calebporzio)**

## Projets communautaires

* [Alpine.js Weekly Newsletter](https://alpinejs.codewithhugo.com/newsletter/)
* [Spruce (State Management)](https://github.com/ryangjchandler/spruce)
* [Turbolinks Adapter](https://github.com/SimoTod/alpine-turbolinks-adapter)
* [Alpine Magic Helpers](https://github.com/KevinBatdorf/alpine-magic-helpers)
* [Awesome Alpine](https://github.com/ryangjchandler/awesome-alpine)

### Directives

---

### `x-data`

**Exemple:** `<div x-data="{ foo: 'bar' }">...</div>`

**Structure:** `<div x-data="[JSON data object]">...</div>`

`x-data` déclares une nouvelle portée de composant. Cela informe le framework qu'il doit initialiser un nouveau composant avec l'objet data spécifié.

Pensez à la propriété `data` des composants VueJs.

**Extraire la logique d'un composant**

Il est possible d'extraire et réutiliser les données (et comportements) dans des functions réutilisable:

```html
<div x-data="dropdown()">
    <button x-on:click="open">Ouvrir</button>

    <div x-show="isOpen()" x-on:click.away="close">
        // Liste déroulante
    </div>
</div>

<script>
    function dropdown() {
        return {
            show: false,
            open() { this.show = true },
            close() { this.show = false },
            isOpen() { return this.show === true },
        }
    }
</script>
```

> **Pour les utilisateurs créant des paquet**, notez que Alpine.js accèdes aux fonctions cituez dans la portée globale(`window`), vous aurez besoin d'assigner les fonctions à l'objet `window` pour pouvoir les utiliser avec `x-data`, par exemple `window.dropdown = function () {}` (Car avec Webpack, Rollup, Parcel etc. Les `function` définies seront pas défaut dans la portée du module et non dans `window`).


Il est également possible de mixer plusieurs objet data via la destructarino d'objet javascript:

```html
<div x-data="{...dropdown(), ...tabs()}">
```

---

### `x-init`
**Exemple:** `<div x-data="{ foo: 'bar' }" x-init="foo = 'baz'"></div>`

**Structure:** `<div x-data="..." x-init="[expression]"></div>`

`x-init` Execute une expression quand le componant à terminer de s'initialiser.

Si vous voulez exécuter du code APRÉS que Alpine.js est fait sa mise à jour initial du DOM (comme le crocher `mounted()` en VueJS), il est possible de retourner un rappel (callback) depuis `x-init`, il sera alors executer apres la mise à jour:

`x-init="() => { // L'état post initialisation du DOM est disponible içi // }"`

---

### `x-show`
**Exemple:** `<div x-show="open"></div>`

**Structure:** `<div x-show="[expression]"></div>`

`x-show` bascule le style `display: none;` de l'élément en fonction de l'expression qui retourne `true` ou `false`.

**x-show.transition**

`x-show.transition` est une aide afin de rendre `x-show` plus plaisant en utilisation des transition CSS.

```html
<div x-show.transition="open">
    Le contenu sera transitioner en entrant et en sortant avec la transition "open".
</div>
```

| Directive | Description |
| --- | --- |
| `x-show.transition` | Simultaner palir et échellonner. (opacity, scale: 0.95, timing-function: cubic-bezier(0.4, 0.0, 0.2, 1), duration-in: 150ms, duration-out: 75ms)
| `x-show.transition.in` | Transition d'entrée uniquement. |
| `x-show.transition.out` | Transition de sortie uniquement. |
| `x-show.transition.opacity` | Plair uniquement. |
| `x-show.transition.scale` | Échellonner uniquement. |
| `x-show.transition.scale.75` | Personnalise la transformation d'échellonage CSS `transform: scale(.75)`. |
| `x-show.transition.duration.200ms` | Assigne la durée de transition d'entrée à 200ms. La transition de sortie se verras assigné la moitié de cette durée (100ms). |
| `x-show.transition.origin.top.right` | Personnaliser l'origine de la transformation `transform-origin: top right`. |
| `x-show.transition.in.duration.200ms.out.duration.50ms` | Durée différente de transition pour l'entrée et la sortie. |

> Note: Tous ces modificateurs de transition peuvent être utiliser ensemble: `x-show.transition.in.duration.100ms.origin.top.right.opacity.scale.85.out.duration.200ms.origin.bottom.left.opacity.scale.95`

> Note: `x-show` attend la fin de toute les transactions de ses enfants. Si vous souhaitez contourner ce fonctionnement, ajouter le modificateur `.immediate`:
```html
<div x-show.immediate="open">
    <div x-show.transition="open">
</div>
```
---

### `x-bind`

> Note: Libre à vous d'utiliser la syntax raccourci ":" -> `:type="..."`

**Exemple:** `<input x-bind:type="inputType">`

**Structure:** `<input x-bind:[attribute]="[expression]">`

`x-bind` assigne la valeur d'un attribut avec le résultat d'une expression Javascript. L'expression a accès à l'ensemble des cléfs de l'objet data du composant, et il sera mis à jour chaque fois que l'objet data est mis à jour.

> Note: les contraintes d'attributs sont mis à jour UNIQUEMENT quand leur dépendances sont mises à jour. Le framework est assez intelligent pour observer les changement de données et detecter quels contraints sont concernées.

**`x-bind` pour l'attribut de class**

`x-bind` se comporte différemment lorsqu'il est associé à l'attribut `class`.

Pour les classes, passez une objet pour lequel les cléfs représenter les noms de classe et ses valeurs sont des expressions booléennes déterminant si les classes respectives sont appliquées.

Par exemple:
`<div x-bind:class="{ 'hidden': foo }"></div>`

Dans cet exemple, la classe "hidden" sera appliquer uniquement quand la valeur data de `foo` est `true`.

**`x-bind` pour les attributs booléens**

`x-bind` supporte les attributs booléens de la même manière que des valeurs d'attributs, utilisant une variable comme condition ou n'importe quelle expression JavaScript resultant à `true` ou `false`.

Par exemple:
```html
<!-- Avec: -->
<button x-bind:disabled="myVar">Cliquez moi</button>

<!-- Quand myVar == true: -->
<button disabled="disabled">Cliquez moi</button>

<!-- Quand myVar == false: -->
<button>Click me</button>
```

Cela va ajouter ou retirer l'attribut `disabled` quand `myVar` est respectivement `true` ou `false`.

Les attributes booléens sont supportés en respectant la [spécification HTML](https://html.spec.whatwg.org/multipage/indices.html#attributes-3:boolean-attribute), par exemple `disabled`, `readonly`, `required`, `checked`, `hidden`, `selected`, `open`, etc.

**Modificateur `.camel`**
**Exemple:** `<svg x-bind:view-box.camel="viewBox">`

Le modificateur `camel` lie la version "camel" du nom de l'attribut. Dans l'exemple précédent, la valeur de `viewBox` sera liée à l'attribute `viewBox` plutot qu'a l'attribut nommé `view-box`.

---

### `x-on`

> Note: Libre à vous d'utiliser la syntax raccourci "@": `@click="..."`

**Exemple:** `<button x-on:click="foo = 'bar'"></button>`

**Structure:** `<button x-on:[event]="[expression]"></button>`

`x-on` attache un écouteur d'événement à l'élément auquel il est attaché. Quand cette événement est émi, l'expression JavaScript spécifié en valeur est alors exécutée.

Si une valeur de data est modifiée dans l'execution de cette expression, les autres attributs liés à celle-ci seront mis à jour.

> Note: Il est également possible de spécifier le nom d'une function JavaScript

**Exemple:** `<button x-on:click="myFunction"></button>`

C'est l'équivalent de: `<button x-on:click="myFunction($event)"></button>`

**Modificateur `keydown`**

**Exemple:** `<input type="text" x-on:keydown.escape="open = false">`

Vous pouvez specifier des cléfs specifique pour le modificateur 'keydown' ajouter a la directive `x-on:keydown`. Notez que les modificateurs sont nommé via le format "kebab" des valeurs `Event.key`.

Exemples: `enter`, `escape`, `arrow-up`, `arrow-down`

> Note: Vous pouvez également écouter des combinaisons de modificateurs système comme: `x-on:keydown.cmd.enter="foo"`

**Modificateur `.away`**

**Exemple:** `<div x-on:click.away="showModal = false"></div>`

Quand le modificateur `.away` est présent, l'écouteur d'événements sera exécuter uniquement si l'évènement est de source autre que lui-meme ou ses enfants.

Cette fonctionnalité est utiles afin de cacher une liste déroulante ou un modale lorsque l'utisateur clique en dehors de ceux-ci.

**Modificateur `.prevent`**
**Exemple:** `<input type="checkbox" x-on:click.prevent>`

Ajouter le modificateur `.prevent` à un écouteur d'événement appelera `preventDefault` sur l'événement déclenché. Dans l'exemple ci-dessus, cela fait que la case à cocher ne sera pas cocher lors du clique utilisateur sur celle-ci.

**Modificateur `.stop`**
**Exemple:** `<div x-on:click="foo = 'bar'"><button x-on:click.stop></button></div>`

Ajouter le modificateur `.stop` à un écouteur d'évènement appellera `stopPropagation` sur l'évènement déclenché. Dans l'exemple ci-dessus, cela fait que l'évènement "click" du bouton ne sera pas propager au `<div>` externe. Ou plutot, quand un utilisateur click sur le bouton, `foo` ne sera pas mis à jour avec la valeur `'bar'`.

**Modificateur `.self`**
**Exemple:** `<div x-on:click.self="foo = 'bar'"><button></button></div>`

Ajouter le modificateur `.self` à un écouteur d'événement appellera le gestionnaire d'évènement saulement si `$event.target` est lui-même l'élément. Dans l'exemple ci-dessus, quand un événement "click" se propage du bouton vers le `<div>` externe, le gestionnaire d'évènement **ne sera pas** excécuter.

**Modificateur `.window`**
**Exemple:** `<div x-on:resize.window="isOpen = window.outerWidth > 768 ? false : open"></div>`

Ajouter le modificateur `.window` à un écouteur d'évènement installera l'écouteur sur l'objet global `window` plutot que sur l'élément DOM sur lequel il est déclaré. Cela est pratique lorsque vous souhaitez modifier l'état d'un composant lorsque quelque chose change avec `window`, comme le changement de taille. Dans l'exemple précédent, lorsque `window` devient plus large de 768px, la modale 

Cela est utile lorsque vous souhaitez modifié l'état d'un componsant lorsque `window` change, comme l'évènement `resize`. Dans l'exemple précédent, la fenêtre dépasse 768 pixels de largeur, nous cloturons la modale/liste déroulane, autrement on garde l'état actuel.

>Note: Il est possible d'utiliser également le modification `.document` pour attacher un écouteur d'évènement à `document` plutot que `window`

**Modificateur `.once`**
**Exemple:** `<button x-on:mouseenter.once="fetchSomething()"></button>`

Ajouter le modificateur d'événement `.once` à un écouteur d'évènement assurera que celui-ci ne soit déclenché qu'une seule fois. Cela permet d'exécuter des actions qu'une seule fois, comme récupérer une vue HTML partielle ou autre.

**Modificateur `.passive`**
**Exemple:** `<button x-on:mousedown.passive="interactive = true"></button>`

Ajouter le modificateur d'évènement `.passive` à un écouteur d'évènement le rendra passif, ce qui fait que `preventDefault()` n'aura aucun effet sur aucun évéènement gérer. Cela est utile, par exemple, avec les performance de défilement des apareils mobiles.

**Modificateur `.debounce`**
**Exemple:** `<input x-on:input.debounce="fetchSomething()">`

Le modificateur d'évènement `debounce` permet de faire 'rebondir' un gestionnaire d'évènements. En d'autres mots, le gestionnaire d'évènement ne sera PAS exécuter tant qu'un certains temps ne se soit écouler depuis le dernier déclenchement de l'évènement. Lorsque d'une gestionnaire d'évènement est prêt a être appelé, le dernier appel du gestionnaire sera exécuté.

La valeur d'attente ('wait') à par défaut une valeur de 250 millisecondes.

Si vous souhaitez modifier celle-ci, spécifié une valeur d'attente comme suit:

```
<input x-on:input.debounce.750="fetchSomething()">
<input x-on:input.debounce.750ms="fetchSomething()">
```

**Modificateur `.camel`**
**Exemple:** `<input x-on:event-name.camel="doSomething()">`

Le modificateur `camel` attache un écouteur d'événement pour le nom d'événement équivalement au format chameau. Dans l'exemple précédent, l'expression sera évaluée lorsque l'événement `eventName` sera déclenché par l'élément.

---

### `x-model`
**Exemple:** `<input type="text" x-model="foo">`

**Structure:** `<input type="text" x-model="[data item]">`

`x-model` ajoute une "liaison de données bidirectionnel" (two-way data binding) à un élément. En d'autres mots, la valeur de saisie de l'élément sera garder synchroniser avec la valeur de l'objet data du composant.

> Note: `x-model` est assez intelligent pour detecter les changements de saisie textes, cases à cocher, boutons radio, zones de texte, selection et selection multiple. Il devrait fonctionne comme [Vue devrait](https://vuejs.org/v2/guide/forms.html) dans les autres scénarios.

**Modificateur `.number`**
**Exemple:** `<input x-model.number="age">`

Le modificateur `number` convertira la valeur seisie en un nombre. Si la valeur ne peut être convertie en un nombre valide, la valeur origianle sera retourné.

**Modificateur `.debounce`**
**Exemple:** `<input x-model.debounce="search">`

Le modificateur `debounce` permet d'ajouter un 'rebondicement' a une mise à jour de valeur. En d'autres mots, le gestionnaire d'évènements ne sera PAS exécuter tant qu'une certaines durée de temps ne se soit passée depuis la dernière émission d'événement. Lorsqu'un gestionnaire est prêt a être appelé, le dernier gestionnaire sera exécuter.

La valeur d'attente par défaut du rebond est 250 millisecondes.

Si vous souhaitez le modifier, vous pouvez spécifier une valeur d'attente personnalisée comme suit:

```
<input x-model.debounce.750="search">
<input x-model.debounce.750ms="search">
```

---

### `x-text`
**Exemple:** `<span x-text="foo"></span>`

**Structure:** `<span x-text="[expression]"`

`x-text` fonctionne de façon similaire à `x-bind`, excepter qu'au lieu de mettre à jour la valeur d'un élément, l'`innerText` de l'élément sera mis à jour.

---

### `x-html`
**Exemple:** `<span x-html="foo"></span>`

**Structure:** `<span x-html="[expression]"`

`x-html` fonctionne de façon similaire à `x-bind`, excepter qu'au lieu de mettre à jour la valeur d'un élément, son `innerHTML` sera mis à jour.

> :warning: **Utiliser uniquement avec un contenu de confiance et jamais avec un contenu renseigné par l'utilisateur.** :warning:
>
> Interprêter du HTML fournis par un tiers peut facilement conduire à des vulnérabilité [XSS](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting).

---

### `x-ref`
**Exemple:** `<div x-ref="foo"></div><button x-on:click="$refs.foo.innerText = 'bar'"></button>`

**Structure:** `<div x-ref="[ref name]"></div><button x-on:click="$refs.[ref name].innerText = 'bar'"></button>`

`x-ref` fournis un façon pratique de récupérer l'élément DOM brut d'un composant. En indiquant un attribute `x-ref` sur un élément, vous le rendez disponible a tous les gestionnaire d'évènement a l'intérieur d'un objet nommé `$refs`.

Cela représente une alternative pour définir des ids et utiliser `document.querySelector` de partout.

> Note: vous pouvez également lié des valeurs dynamique pour x-ref: `<span :x-ref="item.id"></span>` si besoin.

---

### `x-if`
**Exemple:** `<template x-if="true"><div>Some Element</div></template>`

**Structure:** `<template x-if="[expression]"><div>Some Element</div></template>`

Dans les cas où `x-show` n'est pas suffisant (`x-show` fixe un élément à `display: none` si cela vaut false), `x-if` peut être utilisé pour complètement retirer l'élément du DOM.

Il est important que `x-if`soit utiliser sur un tag `<template></template>` car AlpineJs n'utilise pas de DOM virtuel. Cette implémentation permet à AlpineJs de rester robuste et d'utiliser le DOM réel pour performer sa magie.

> Note: `x-if` doit avoir un seul élément racine a l'intérieur du tag `<template></template>`.

> Note: Lorsque l'on utilise un `template` dans un tag `svg`, il ajouter un [polyfill](https://github.com/alpinejs/alpine/issues/637#issuecomment-654856538) qui s'exécute avant que AlpineJs soit initialisé.

---

### `x-for`
**Exemple:**
```html
<template x-for="item in items" :key="item">
    <div x-text="item"></div>
</template>
```

> Note: la contrainte `:key` est optionnel, may FORTEMENT recommandé.

`x-for` est disponible pour les cas ou l'on souhaite créer un nouvel élément DOM pour chaque objet d'un tableau. Similaire au `v-for`de VueJs, à l'exception qu'il doit exister sur un tag `template`, and non sur un tag régulier du DOM.

Si vous souhaitez accéder à l'index courant de l'iteration, utiliser la syntax suivante:

```html
<template x-for="(item, index) in items" :key="index">
<!-- Vous pouvez également référencer "index" a l'interieur de l'iteration si besoin. -->
    <div x-text="index"></div>
</template>
```

> Note: `x-for` doit avoir un seul élément racine dans le tag `<template></template>`.

> Note: Lorsque l'on utilise un `template` dans un tag `svg`, il ajouter un [polyfill](https://github.com/alpinejs/alpine/issues/637#issuecomment-654856538) qui s'exécute avant que AlpineJs soit initialisé.

#### `x-for` imbriqués
vous pouvez imbriquer les boucles `x-for`, mais vous DEVEZ englober chacune dans un élément. Par exemple:

```html
<template x-for="item in items">
    <div>
        <template x-for="subItem in item.subItems">
            <div x-text="subItem"></div>
        </template>
    </div>
</template>
```

#### Itérer sur une intervalle

AlpineJs support la syntaxe `i in n`, où `n` est un entier, permettant d'itérer sur une intervall fixe d'éléments.

```html
<template x-for="i in 10">
    <span x-text="i"></span>
</template>
```

---

### `x-transition`
**Exemple:**
```html
<div
    x-show="open"
    x-transition:enter="transition ease-out duration-300"
    x-transition:enter-start="opacity-0 transform scale-90"
    x-transition:enter-end="opacity-100 transform scale-100"
    x-transition:leave="transition ease-in duration-300"
    x-transition:leave-start="opacity-100 transform scale-100"
    x-transition:leave-end="opacity-0 transform scale-90"
>...</div>
```

```html
<template x-if="open">
    <div
        x-transition:enter="transition ease-out duration-300"
        x-transition:enter-start="opacity-0 transform scale-90"
        x-transition:enter-end="opacity-100 transform scale-100"
        x-transition:leave="transition ease-in duration-300"
        x-transition:leave-start="opacity-100 transform scale-100"
        x-transition:leave-end="opacity-0 transform scale-90"
    >...</div>
</template>
```

> L'exemple ci-dessus utiliser des classes de [Tailwind CSS](https://tailwindcss.com)

AlpineJs offre 6 directives de transitions pour appliquer des classes à différents états de transition d'élément entre les états "hidden" et "shown". Ces directives fonctionnent avec à la fois `x-show` ET `x-if`.

Elles se comportent éxactement comme les directives de transition VueJs, à l'exception de leur appelation:

| Directive | Description |
| --- | --- |
| `:enter` | Appliqué pendant la phase d'entrée. |
| `:enter-start` | Ajouté avant qu'un élément soit inséré, retiré une frame aprés qu'il soit inséré. |
| `:enter-end` | Ajouté une frame aprés que l'élément soit inséré ( au même moment que `enter-start` est retiré), retiré quand les transitions/animations finisse.
| `:leave` | Appliqué pendant toute la phase de sortie. |
| `:leave-start` | Ajouté immédiatement aprés qu'un départ soit déclenché, retiré aprés une frame. |
| `:leave-end` | Ajouté une frame aprés qu'un départ soit déclenché (au même moment qur `leave-start` est retiré), retiré quand les transitions/animations finisse.

---

### `x-spread`
**Exemple:**
```html
<div x-data="dropdown()">
    <button x-spread="trigger">Ouvrir liste déroulante</button>

    <span x-spread="dialogue">Contenu de la liste déroulante</span>
</div>

<script>
    function dropdown() {
        return {
            open: false,
            trigger: {
                ['@click']() {
                    this.open = true
                },
            },
            dialogue: {
                ['x-show']() {
                    return this.open
                },
                ['@click.away']() {
                    this.open = false
                },
            }
        }
    }
</script>
```

`x-spread` permet d'extraire les contraintes d'un élément AlpineJs dans un objet réutilisable.

Les clefs de l'objet soit les directives (peuvent être n'importe qu'elle directive incluant les modificateurs), et les valeurs sont les fonctions de rappel executées par AlpineJs.

> Note: L'unique anomalie avec `x-spread` est quand utilisé avec `x-for`. Quand la directive étant propagé est un `x-for`, vous pouvez retourner une expression normal de chaine de caractères depuis la fonction de rappel. Par exemple: `['x-for']() { return 'item in items' }`.

---

### `x-cloak`
**Exemple:** `<div x-data="{}" x-cloak></div>`

Les attributs `x-cloak` sont retirés des éléments quand AlpineJs s'initialise. C'est pratiquement utile pour caché des élément de DOM pré-initialiser. C'est typique d'ajouter ce style global pour que cela fonctionne:

```html
<style>
    [x-cloak] { display: none; }
</style>
```

### Propriétés magiques

> Avec l'exception de `$el`, les propriétés magiques de **ne sont pas disponibles à l'intérieur de `x-data`** car le composant n'est pas encore initialisé.

---

### `$el`
**Exemple:**
```html
<div x-data>
    <button @click="$el.innerHTML = 'foo'">Remplace moi par "foo"</button>
</div>
```

`$el` est propriété magique qui peut être utilisée afin de récupérer le noeud du DOM racine.

### `$refs`
**Exemple:**
```html
<span x-ref="foo"></span>

<button x-on:click="$refs.foo.innerText = 'bar'"></button>
```

`$refs` est propriété magique qui peut être utilisée pour récupérer les éléments du DOM marqués avec `x-ref` à l'intérieur d'un composant. C'est pratique quand vous avez besoin de manipuler certains élément du DOM.

---

### `$event`
**Exemple:**
```html
<input x-on:input="alert($event.target.value)">
```

`$event` est une propriété magique permettant d'accèder, à l'intérieur d'un écouteur d'événement, à l'objet "Event" natif du navigateur..

> Note: La propriété $event est uniquement disponible dans les expressions DOM.

Si vous avez besoin d'accéder à $event dans une fonction JavaScript, vous pouvez le passer directement:

`<button x-on:click="myFunction($event)"></button>`

---

### `$dispatch`
**Exemple:**
```html
<div @custom-event="console.log($event.detail.foo)">
    <button @click="$dispatch('custom-event', { foo: 'bar' })">
    <!-- Quand cliquer, console.log "bar" -->
</div>
```

**Note sur la Propagation d'événements**

Notez que, à cause de [event bubbling](https://en.wikipedia.org/wiki/Event_bubbling), quand vous avez besoin de capturer des évènements transmis depuis des noeuds qui sont sous la meme hiérarchie d'imbrication, vous aurez besoin d'utiliser le modifieur [`.window`](https://github.com/alpinejs/alpine#x-on):

**Exemple:**

```html
<div x-data>
    <span @custom-event="console.log($event.detail.foo)"></span>
    <button @click="$dispatch('custom-event', { foo: 'bar' })">
<div>
```

> Cela ne fonctionnera pas car quand `custom-event` est transmis, il va se propagé vers son ancêtre commun, `div`.

**Déclenher vers des Composants**

vous pouvez également utilisé la technique précédente pour faire que les composants intéragissent entre eux:

**Exemple:**

```html
<div x-data @custom-event.window="console.log($event.detail)"></div>

<button x-data @click="$dispatch('custom-event', 'Hello World!')">
<!-- Quand cliqué, console.log "Hello World!". -->
```

`$dispatch` est un raccourci pour créer un `CustomEvent` et le transmettre via `.dispatchEvent()` intérieurement. Il y a pleins de bon case d'utilisation pour passer des données entre composants via des évènements. [Lire plus](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events) pour plus d'information sur le système sous-jacent de  `CustomEvent` dans les navigateurs.

Vous remarquerez que toute données passée comme second paramèetre à `$dispatch('some-event', { some: 'data' })`, devient disponible au travers de la propriété détail du nouvel événement: `$event.detail.some`. Attacher des données à la propriété `.detail` d'un événement est une pratique standard pour les événements `CustomEvent` des navigateurs. [Lire plus](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/detail) pour plus d'informations.

Vous pouvez également utiliser `$dispatch()` pour déclencher des mise à jour de données pour `x-model`. Par exemple:

```html
<div x-data="{ foo: 'bar' }">
    <span x-model="foo">
        <button @click="$dispatch('input', 'baz')">
        <!-- Aprés que le bouton soit cliqué, `x-model` va attrapé l'événement 'input', et mettre à jour foo avec "baz". -->
    </span>
</div>
```

> Note: La propriété `$dispatch` est uniquement disponible dans les expression DOM.

Si vous avez besoin d'accéder à `$dispatch` a l'intérieur d'une fonction JavaScript vous pouvez la passer directement:

`<button x-on:click="myFunction($dispatch)"></button>`

---

### `$nextTick`
**Exemple:**
```html
<div x-data="{ fruit: 'apple' }">
    <button
        x-on:click="
            fruit = 'pear';
            $nextTick(() => { console.log($event.target.innerText) });
        "
        x-text="fruit"
    ></button>
</div>
```

`$nextTick` est une propriété magique qui permet d'executer une expression uniquement APRÈS que AlpineJs ai fait ses mise à jour interactive du DOM. C'est utile lorsque l'on souhaite intéragir avec l'état du DOM aprés qu'il ai pris en compte les mise à jour de données entrepris.

---

### `$watch`
**Exemple:**
```html
<div x-data="{ open: false }" x-init="$watch('open', value => console.log(value))">
    <button @click="open = ! open">Toggle Open</button>
</div>
```
Vous pouvez "watch" une propriété d'un composant via la méthode magique `$watch`. Dans l'exemple ci-dessous, quand un bouton est cliqué et `open` à changé, le rappel fournis sera executé et `console.log` la nouvelle valeur.

## Securité
Si vous trouvez une vulnérabilité de sécurité, merci d'envoyer un email à [calebporzio@gmail.com]()

Alpine se base sur une implémentation maison utilisant l'objet `Function` pour évaluer ses directives. Malgrés que cela soit plus sécurisé que `eval()`, son utilisation est défendue dans certains environnements comme Google Chrome App, utilisant une restriction Content Security Policy (CSP).

Si vous utilisé AlpineJs sur une site web utilisant des données sensible et que vous avez besoin de [CSP](https://csp.withgoogle.com/docs/strict-csp.html), vous aurez besoin d'inclure  `unsafe-eval` dans votre police. Une police robuste correctement configurée aidera à protéger vos utilisateurs lorsqu'il renseigne des données personnelles ou financiaires..

Comme une police s'applique a l'ensemble des scripts d'une page, il est important que les autres librairies externes au site soit évalué avec précaution pour s'assurer qu'elles sont de confiances et qu'elles ne vont pas introduire des vulnérabilités de type Cross Site Scripting, que ce soit par l'utilisation de `eval()` ou par manipulation du DOM pour injecter du code malintentionné dans votre page.

## V3 Roadmap
Voir le README principal

## License

Copyright © 2019-2020 Caleb Porzio and contributors

Licensed under the MIT license, see [LICENSE.md](LICENSE.md) for details.
