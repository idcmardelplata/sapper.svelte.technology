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

> `delete` es una palabra reservada en JavaScript. Para manejar peticiónes DELETE, exporte una función llamada `del` en su lugar.

Hay tres simples reglas para nombrar los archivos que definen sus rutas:

* Un archivo llamado `routes/about.html` corresponde con la ruta `/about`. Un arhivo llamado `routes/blog/[slug].html` corresponde con la ruta `/blog/:slug`, en ese caso la propiedad `params.slug` esta disponible para la ruta
* El archivo `routes/index.html` (o `routes/index.js`) corresponde a la raíz de su aplicación, `routes/about/index.html` es tratado igual que `routes/about.html`.
* Los archivos y directorios con un subrayado inicial *no* crean rutas. Esto le permite colocar modulos auxiliares y componentes con las rutas que dependen de ellos - por ejemplo podria tener un archivo llamado `routes/_helpers/datetime.js` y *no* se crearia una ruta a `/_helpers/datetime`.

### Subroutes

Suponga que tiene una ruta llamada  `/settings` y una serie de subrutas como  `/settings/profile` y  `/settings/notifications`.

Puede hacer esto con dos páginas separadas - `routes/settings.html` (o `routes/settings/index.html`) y `routes/settings/[submenu].html`, pero es probable que compartan una gran cantidad de código. Si no hay un archivo `routes/settings.html`, entonces `routes/settings/[submenu].html` coincidira con `/settings` asi como también con subrutas como `/settings/profile`, pero el valor de `params.submenu` sera `undefined` en el primer caso.


### Error pages

Ademas e las paginas regulares, existen dos páginas 'especiales' - `routes/4xx.html` y `routes/5xx.html`. Estas se mostraran cuando se produzca un error al renderizar una página.

El objeto `error` estara disponible en la plantilla.

* **4xx.html** se muestra cuando se encuentra un error con un código de estado HTTP [entre 400 y 499](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#4xx_Client_errors), como 404 Página no encontrada
* **5xx.html** es mostrada para [todos los otros errores](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_Server_errors). En la practica, esto significa 500 Internal Server Error.
