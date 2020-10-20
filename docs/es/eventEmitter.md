# Event emitter

Se expone un [event emitter](https://nodejs.org/api/events.html) por si se quisiera saber más sobre lo que está pasando mientras se parsean los comentarios o el resultado de nuestro swagger.

Estos son los eventos que se exponen:

- **error** puedes suscribirte a este evento para depurar si hay un problema mientras se ejecuta el proceso de análisis.
- **proceso** puedes suscribirte a este evento para depurar cómo se analizan los comentarios de jsdoc en el objeto de swagger por entidad.
- **finalizar** puedes suscribirte para recibir el resultado del objeto de arrogancia generado.

## Example

```javascript
  
const express = require('express');
const expressJSDocSwagger = require('express-jsdoc-swagger');

const options = {
  info: {
    version: '1.0.0',
    title: 'Albums store',
    license: {
      name: 'MIT',
    },
  },
  filesPattern: './simple.js',
  baseDir: __dirname,
};

const app = express();
const port = 3000;

const listener = expressJSDocSwagger(app)(options);

// Event emitter API
listener.on('error', error => {
  console.error(`Error: ${error}`);
});

listener.on('process', ({ entity, swaggerObject }) => {
  console.log(`entity: ${entity}`);
  console.log('swaggerObject');
  console.log(swaggerObject);
});

listener.on('finish', swaggerObject => {
  console.log('Finish');
  console.log(swaggerObject);
});

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`));
````

> Puedes ver más ejemplos [aquí](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/eventEmitter).

