# Seguridad
Para añadir [Autenticación](https://swagger.io/docs/specification/authentication/) a tus endpoints con `express-jsdoc-swagger`, tienes que añadir la opción `security` en las opciones de configuración cuando se crea la instancia.

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
  security: {
    BasicAuth: {
      type: 'http',
      scheme: 'basic',
    },
  },
  filesPattern: './basic-auth.js',
  baseDir: __dirname,
};

const app = express();
const port = 3000;

expressJSDocSwagger(app)(options);
```

- `Security` se usa para definir tus la configuración de seguridad, y es opcional por si no se quisiera ningún tipo de seguridad.

Después de agregar la opción `security`, se puede usar en los endpoints de la siguiente manera:

```javascript
/**
 * GET /api/v1
 * @summary This is the summary or description of the endpoint
 * @return {string} 200 - success response
 * @security BasicAuth
 */
app.get('/api/v1', (req, res) => res.send('Hello World!'));
```

El swagger resultante será:

<img src="./assets/security.png"/>

## Varios tipos de autenticación

Puedes utilizar varios sistemas de autenticación y combinarlos con operadores lógicos como AND (`&`) o OR (`|`) para obtener el resultado deseado:

```javascript
const options = {
  info: {
    version: '1.0.0',
    title: 'Albums store',
    license: {
      name: 'MIT',
    },
  },
  security: {
    BasicAuth: {
      type: 'http',
      scheme: 'basic',
    },
    BearerAuth: {
      type: 'http',
      scheme: 'bearer',
    },
  },
  filesPattern: './basic-auth.js',
  baseDir: __dirname,
};

/**
 * GET /api/v2
 * @summary Endpoint with multiple security configuration (AND logic)
 * @return {string} 200 - success response
 * @security BasicAuth & BearerAuth
 */
app.get('/api/v2', (req, res) => res.send('Hello World!'));

/**
 * GET /api/v3
 * @summary Endpoint with multiple security configuration (OR logic)
 * @return {string} 200 - success response
 * @security BasicAuth | BearerAuth
 */
app.get('/api/v3', (req, res) => res.send('Hello World!'));
```

> Puedes ver más ejemplos [aquí](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/security/basic-auth.js).
