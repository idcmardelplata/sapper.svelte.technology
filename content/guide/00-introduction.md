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

* Sapper funciona con Svelte en lugar de React, por lo tanto es mas rapido y sus aplicaciónes mas pequeñas.
* En lugar de enmascarar las rutas, codificamos los parametros de ruta en el nombre de archivo (vea la seccion [routing](#routing) )
* Ademas de *paginas*, puede crear *rutas de servidor* en su directorio `routes`. Esto hace que sea muy facil, por ejemplo, agregar una api JSON como la que utiliza esta página ( pruebe visitando [/guide.json](/guide.json))
* Los enlaces son solo elementos `<a>`, en lugar de los componentes especificos `<Link>` del framework. Eso significa, que [este enlace de aqui](/), a pesar de estar dentro de una burbuja de markdown, funcióna con el router como es de esperarse.

### Comenzando

La forma más facil de comenzar a construir una aplicación Sapper es clonar el repositorio de  [sapper-template](https://github.com/sveltejs/sapper-template) con  [degit](https://github.com/Rich-Harris/degit):

```js
npm install -g degit
degit sveltejs/sapper-template my-app
cd my-app
npm install
npm run dev
```

Esto creara el scaffold de un nuevo proyecto en el directorio `my-app`, instala las dependencias, y arrancara un servidor en [localhost:3000](http://localhost:3000). Intente editar los archivos para tener una idea de como funciona todo - es posible que no necesites molestarte leyendo el resto de esta guia!.
