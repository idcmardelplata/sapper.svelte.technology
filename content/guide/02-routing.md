---
title: Routing
---

Como hemos visto, existen dos tipos de rutas en Sapper - paginas y rutas del servidor.


### Paginas

Las paginas son componentes de Svelte escritos en archivos `.html`. Cuando un usuario visita por primera vez la aplicación, se le servira una versión de la ruta en cuestion renderizada en el lado del servidor, ademas de un poco de código JavaScript que 'hidrata' la página e inicializa un enrutador en el lado del cliente. Desde ese punto en adelante, la navegación hacia otras paginas se maneja enteramente en el cliente para una sensación rapida similar a una aplicación.

Por ejemplo, asi es como podria crear una página para que renderice un post de un blog:

```html
<!-- routes/blog/[slug].html -->
<:Head>
	<title>{{post.title}}</title>
</:Head>

<h1>{{post.title}}</h1>

<div class='content'>
	{{{post.html}}}
</div>

<script>
	export default {
		// the (optional) preload function takes a
		// `{ params, query }` object and turns it into
		// the data we need to render the page
		preload({ params, query }) {
			// the `slug` parameter is available because this file
			// is called [slug].html
			const { slug } = params;

			return fetch(`/blog/${slug}.json`).then(r => r.json()).then(post => {
				return { post };
			});
		}
	};
</script>
```

> Cuando renderiza paginas en el servidor, la función `preload` recive el objeto `request` completo, que suele incluir las propiedades `params` y `query`. Esto le permite usar [middleware de sesión](https://github.com/expressjs/session) (por ejemplo). En el cliente, solo las propiedades `params` y `query` son provistas. Consulte la sección  [preloading](#preloading) para más información.

### Server routes

Las rutas de servidor son modulos escritos en archivos `.js` que exportan funciones correspondientes a los metodos HTTP. Cada función recive los objetos Express  `request` y `response` como argumentos, mas una función `next`. Esto es útil para crear una API JSON. Por ejemplo, asi es como podria crear un endpoint la pagina del blog de arriba:

```js
// routes/blog/[slug].json.js
import db from './_database.js'; // the underscore tells Sapper this isn't a route

export async function get(req, res, next) {
	// the `slug` parameter is available because this file
	// is called [slug].json.js
	const { slug } = req.params;

	const post = await db.get(slug);

	if (post !== null) {
		res.set('Content-Type', 'application/json');
		res.end(JSON.stringify(post));
	} else {
		next();
	}
}
```

> `delete` is a reserved word in JavaScript. To handle DELETE requests, export a function called `del` instead.

There are three simple rules for naming the files that define your routes:

* A file called `routes/about.html` corresponds to the `/about` route. A file called `routes/blog/[slug].html` corresponds to the `/blog/:slug` route, in which case `params.slug` is available to the route
* The file `routes/index.html` (or `routes/index.js`) corresponds to the root of your app. `routes/about/index.html` is treated the same as `routes/about.html`.
* Files and directories with a leading underscore do *not* create routes. This allows you to colocate helper modules and components with the routes that depend on them — for example you could have a file called `routes/_helpers/datetime.js` and it would *not* create a `/_helpers/datetime` route



### Subroutes

Suppose you have a route called `/settings` and a series of subroutes such as `/settings/profile` and `/settings/notifications`.

You could do this with two separate pages — `routes/settings.html` (or `routes/settings/index.html`) and `routes/settings/[submenu].html`, but it's likely that they'll share a lot of code. If there's no `routes/settings.html` file, then `routes/settings/[submenu].html` will match `/settings` as well as subroutes like `/settings/profile`, but the value of `params.submenu` will be `undefined` in the first case.



### Error pages

In addition to regular pages, there are two 'special' pages — `routes/4xx.html` and `routes/5xx.html`. These will be shown when an error occurs while rendering a page.

The `error` object is made available to the template.

* **4xx.html** is shown when an error is encountered with an HTTP status code [between 400 and 499](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#4xx_Client_errors), such as 404 Not Found
* **5xx.html** is shown for [all other errors](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_Server_errors). In practice, this means 500 Internal Server Error.
