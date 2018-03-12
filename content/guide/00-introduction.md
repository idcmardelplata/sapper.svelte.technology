---
title: Introduction
---

*Nota: Sapper esta en desarrollo temprano, y algunas cosas pueden cambiar antes de llegar a la version 1.*

### Que es Sapper?

Sapper es un framework para construir aplicaciónes web de alto rendimiento. Estas viendo uno ahora mismo! Hay dos conceptos básicos:

* Cada pagina de su aplicación es un componente de [Svelte](https://svelte.technology) 
* Puede crear páginas agregando archivos al directorio `routes` de su proyecto. Estos seran renderizados en el servidor para que en la primera visita de un usuario a su aplicación sea lo mas rapida posible, luego una aplicación en el lado del cliente se hara cargo.

Construir una aplicación con todas las mejores practicas modernas - code-splitting, soporte offline, vistas renderizadas en el servidor con hidratación en el lado del cliente - es diabolicamente complicado. Sapper hace todas las cosas aburridas para que puedas seguir con la parte creativa.

No necesita saber Svelte para entender el resto de esta guia, pero ayudara. En resumen se trata de un framework UI que compila sus componentes para javascript altamente optimizado. Lea el [Post introductorio](https://svelte.technology/blog/frameworks-without-the-framework) y la [guia](https://svelte.technology/guide) para conocer más.


### Porque el nombre?

En la guerra, los soldados que construyen puentes, reparan caminos, limpian campos minados y conducen demoliciónes - todo bajo condiciónes de combate - son conocidos como *sappers* (zapadores).

Para desarrolladores web, las apuestas son generalmente más bajas que para los ingenieros de combate. Pero tambien enfrentamos nuestro propio entorno hostil: dispositivos de poca potencia, conexiones de red deficientes, y la complejidad inherente a la ingenieria de front-end. Sapper, que es la abreviatura de **S**velte **app** mak**er**, es tu aliado valiente y obediente.

### Comparación con Next.js

[Next.js](https://github.com/zeit/next.js) es un framework para React de [Zeit](https://zeit.co), y es la inspiración para Sapper. Sin embargo hay algunas diferencias notables:

* Sapper is powered by Svelte instead of React, so it's faster and your apps are smaller
* Instead of route masking, we encode route parameters in filenames (see the [routing](#routing) section below)
* As well as *pages*, you can create *server routes* in your `routes` directory. This makes it very easy to, for example, add a JSON API such as the one powering this very page (try visiting [/guide.json](/guide.json))
* Links are just `<a>` elements, rather than framework-specific `<Link>` components. That means, for example, that [this link right here](/), despite being inside a blob of markdown, works with the router as you'd expect


### Getting started

The easiest way to start building a Sapper app is to clone the [sapper-template](https://github.com/sveltejs/sapper-template) repo with [degit](https://github.com/Rich-Harris/degit):

```js
npm install -g degit
degit sveltejs/sapper-template my-app
cd my-app
npm install
npm run dev
```

This will scaffold a new project in the `my-app` directory, install its dependencies, and start a server on [localhost:3000](http://localhost:3000). Try editing the files to get a feel for how everything works – you may not need to bother reading the rest of this guide!
