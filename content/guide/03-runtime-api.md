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

Navega programaticamente a la dirección del `href` dado. Si el destino es una ruta de Sapper, Sapper se encargara de la navegación, de lo contrario, la página se volvera a cargar con el nuevo `href`. (en otras palabras, el comportamiento es como si el usuario hiciera click en un enlace con este `href`.)

### prefetch(href)

* `href` — La página a precargar

Programador previamente busca la página data,lo que significa a) se asegura que el código de la página este cargado, b) llamando al metodo `preload` de la página con las opciónes apropiadas. Este es el mismo comportamiento que Sapper desencadena  cuando el usuario toca o mueve el mouse sobre un elemento `<a>` con [rel=prefetch](#prefetching).


### prefetchRoutes([routes])

* `routes` — un array opcional de strings representando las rutas a precargar.

Programado previamente busca el codigo de las rutas que ahún no han sido buscadas. Por lo general, puede llamar a esto despues de que `init` este completo, para acelerar la subsecuente navegación. La omisión de los argumentos ara que se obtengan tods las rutas, o puede especificar rutas por cualquier nombre de ruta coincidente como `/about` (que coincide con `/routes/about.html`) o `/blog/*` (que coincide con `routes/blog/[slug].html`). A diferencia de `prefetch`, esot no llamara a `preload` para páginas individuales.
