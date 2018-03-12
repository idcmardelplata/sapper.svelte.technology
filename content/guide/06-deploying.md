---
title: Deployment
---

Las aplicaciónes Sapper pueden ejecutarse en cualquier lugar que soporte Node.js 8 o superior.


### Desplegando con Now

Podemos hacer deploy de nuestras aplicaciónes de una manera muy sencilla con [Now](https://zeit.co/now):
We can very easily deploy our apps to [Now](https://zeit.co/now):

```bash
npm install -g now
now
```

Esto subira el código fuente a Now, con lo cual luego necesitara hacer `npm run build` y `npm start` y darle una URL para la aplicación desplegada.

Para otros entornos de hosting, es posible que deba hacer `npm run build` usted mismo.
