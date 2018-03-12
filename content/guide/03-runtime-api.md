---
title: Runtime API
---

El modulo `sapper/runtime.js` contiene funciones para controlar su aplicación y responder a eventos. Mas funciones seran agregadas en futuras versiones.

### init(selector, routes)

* `selector` — un string representando el elemento para renderizar páginas, tipicamente`'#sapper'`
* `routes` — un array de objetos de rutas generados por Sapper

Esto configura el enrutador e inicia la aplicación - escucha por clicks en los elementos `<a>`, interactua con el API del `historial`, y renderiza y actualiza sus componentes Svelte

Retorna una `Promise` que se resuelve cuando la página inicial haya sido hidratada.

```js
import { init } from 'sapper/runtime.js';
import { routes } from './manifest/client.js';

init('#sapper', routes).then(() => {
	console.log('client-side app has started');
});
```


### goto(href, [options])

* `href` — la página para ir a
* `options` — puede incluir una propiedad `replaceState`, que determina si usar `history.pushState` (por defecto) o `history.replaceState`). No requerido.


Programmatically navigates to the given `href`. If the destination is a Sapper route, Sapper will handle the navigation, otherwise the page will be reloaded with the new `href`. (In other words, the behaviour is as though the user clicked on a link with this `href`.)


### prefetch(href)

* `href` — the page to prefetch

Programmatically prefetches the given page, which means a) ensuring that the code for the page is loaded, and b) calling the page's `preload` method with the appropriate options. This is the same behaviour that Sapper triggers when the user taps or mouses over an `<a>` element with [rel=prefetch](#prefetching).



### prefetchRoutes([routes])

* `routes` — an optional array of strings representing routes to prefetch

Programmatically prefetches the code for routes that haven't yet been fetched. Typically, you might call this after `init` is complete, to speed up subsequent navigation. Omitting arguments will cause all routes to be fetched, or you can specify routes by any matching pathname such as `/about` (to match `routes/about.html`) or `/blog/*` (to match `routes/blog/[slug].html`). Unlike `prefetch`, this won't call `preload` for individual pages.
