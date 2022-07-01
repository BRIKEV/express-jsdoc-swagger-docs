![npm](https://img.shields.io/npm/v/express-jsdoc-swagger)
![Node.js Package](https://github.com/BRIKEV/express-jsdoc-swagger/workflows/Build/badge.svg)
[![Known Vulnerabilities](https://snyk.io/test/github/BRIKEV/express-jsdoc-swagger/badge.svg)](https://snyk.io/test/github/BRIKEV/express-jsdoc-swagger)
[![Maintainability](https://api.codeclimate.com/v1/badges/6d5565df0c9c10e75b59/maintainability)](https://codeclimate.com/github/BRIKEV/express-jsdoc-swagger/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/6d5565df0c9c10e75b59/test_coverage)](https://codeclimate.com/github/BRIKEV/express-jsdoc-swagger/test_coverage)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
![npm](https://img.shields.io/npm/dm/express-jsdoc-swagger)

# express-jsdoc-swagger

Con `express-jsdoc-swagger`, puedes documentar tus endpoints usando la [especificación OpenAPI 3 de swagger](https://swagger.io/specification/) sin escribir YAML o JSON. Sólo tienes que escribir comentarios en tus endpoints y la interfaz de usuario se generará automáticamente.

## Prerrequisitos
Se asume que se está usando:
1. [NodeJS](https://nodejs.org)
2. [Express.js](http://www.expressjs.com)

## Instalaión
```
npm i express-jsdoc-swagger
```

## Uso

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
  baseDir: __dirname,
  // Glob pattern to find your jsdoc files (multiple patterns can be added in an array)
  filesPattern: './**/*.js',
  // URL where SwaggerUI will be rendered
  swaggerUIPath: '/api-docs',
  // Expose OpenAPI UI
  exposeSwaggerUI: true,
  // Expose Open API JSON Docs documentation in `apiDocsPath` path.
  exposeApiDocs: false,
  // Open API JSON Docs endpoint.
  apiDocsPath: '/v3/api-docs',
  // Set non-required fields as nullable by default
  notRequiredAsNullable: false,
  // You can customize your UI options.
  // you can extend swagger-ui-express config. You can checkout an example of this
  // in the `example/configuration/swaggerOptions.js`
  swaggerUiOptions: {},
};

const app = express();
const PORT = 3000;

expressJSDocSwagger(app)(options);

/**
 * GET /api/v1
 * @summary This is the summary or description of the endpoint
 * @return {object} 200 - success response
 */
app.get('/api/v1', (req, res) => res.json({
  success: true,
}));

app.listen(PORT, () => console.log(`Example app listening at http://localhost:${PORT}`));
```

## Ejemplos

1. Configuración básica

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
  },
  filesPattern: './**/*.js', // Glob pattern to find your jsdoc files
  baseDir: __dirname,
};
```

2. Definición de los componentes

```javascript
/**
 * A song type
 * @typedef {object} Song
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {number} year - The year - double
 */
```

3. Endpoint que retorna un array de `Songs`

```javascript
/**
 * GET /api/v1/albums
 * @summary This is the summary or description of the endpoint
 * @tags album
 * @return {array<Song>} 200 - success response - application/json
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'abum 1',
  }])
));
```

4. Definición básica de un endpoint con tags, parámetros y una autenticación básica

```javascript
/**
 * GET /api/v1/album
 * @summary This is the summary or description of the endpoint
 * @security BasicAuth
 * @tags album
 * @param {string} name.query.required - name param description
 * @return {object} 200 - success response - application/json
 * @return {object} 400 - Bad request response
 */
app.get('/api/v1/album', (req, res) => (
  res.json({
    title: 'abum 1',
  })
));
```

5. Endpoint básico con ejemplo del cuerpo de una respuesta

```javascript
/**
 * GET /api/v1/albums
 * @summary This is the summary or description of the endpoint
 * @tags album
 * @return {array<Song>} 200 - success response - application/json
 * @example response - 200 - success response example
 * [
 *   {
 *     "title": "Bury the light",
 *     "artist": "Casey Edwards ft. Victor Borba",
 *     "year": 2020
 *   }
 * ]
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'track 1',
  }])
));
```

Puedes encontrar más ejemplos [aquí](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples).

## Validator

Hemos desarrollado un módulo que sirve como validador de los Endpoints que se documentan con `express-jsdoc-swagger`. Este módulo es [express-oas-validator](https://github.com/BRIKEV/express-oas-validator).

**Example**

Instalar usando npm:

```
npm install --save express-oas-validator
```

Después es necesario hacer init con la config de swagger al terminar el evento `finish`. Más información sobre esto en esta [sección](eventEmitter.md).

```js
const instance = expressJSDocSwagger(app)(options);

instance.on('finish', data => {
  init(data);
  resolve(app);
});
```

Ejemplo completo de como integrarlo con `express-jsdoc-swagger`.

```js
const express = require('express');
const expressJSDocSwagger = require('express-jsdoc-swagger');
const { init, validateRequest, validateResponse } = require('express-oas-validator');

const options = {
  info: {
    version: '1.0.0',
    title: 'Albums store',
    license: {
      name: 'MIT',
    },
  },
  filesPattern: './**.js',
  baseDir: __dirname,
};

const app = express();
const instance = expressJSDocSwagger(app)(options);

const serverApp = () => new Promise(resolve => {
  instance.on('finish', data => {
    init(data);
    resolve(app);
  });

  app.use(express.urlencoded({ extended: true }));
  app.use(express.json());

  /**
   * A song
   * @typedef {object} Song
   * @property {string} title.required - The title
   * @property {string} artist - The artist
   * @property {integer} year - The year
   */

  /**
   * POST /api/v1/songs
   * @param {Song} request.body.required - song info
   * @return {object} 200 - song response
   */
  app.post('/api/v1/songs', validateRequest(), (req, res) => res.send('You save a song!'));

  /**
   * POST /api/v1/name
   * @param {string} request.body.required - name body description
   * @return {object} 200 - song response
   */
  app.post('/api/v1/name', (req, res, next) => {
    try {
      // Validate response
      validateResponse('Error string', req);
      return res.send('Hello World!');
    } catch (error) {
      return next(error);
    }
  });

  /**
   * GET /api/v1/authors
   * @summary This is the summary or description of the endpoint
   * @param {string} name.query.required - name param description - enum:type1,type2
   * @param {array<string>} license.query - name param description
   * @return {object} 200 - success response - application/json
   */
  app.get('/api/v1/authors', validateRequest({ headers: false }), (req, res) => (
    res.json([{
      title: 'abum 1',
    }])
  ));

  // eslint-disable-next-line no-unused-vars
  app.use((err, req, res, next) => {
    res.status(err.status).json(err);
  });
});

module.exports = serverApp;
```

## Contributors ✨

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/bri06"><img src="https://avatars0.githubusercontent.com/u/24435223?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Briam Martinez Escobar</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=bri06" title="Code">💻</a></td>
    <td align="center"><a href="https://twitter.com/kjmesc"><img src="https://avatars2.githubusercontent.com/u/12685053?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kevin Julián Martínez Escobar</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=kevinccbsg" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/hoonga"><img src="https://avatars3.githubusercontent.com/u/10708927?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Heung-yeon Oh</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=hoonga" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/LonelyPrincess"><img src="https://avatars1.githubusercontent.com/u/17673317?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sara Hernández</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=LonelyPrincess" title="Code">💻</a></td>
    <td align="center"><a href="http://servatj.me"><img src="https://avatars0.githubusercontent.com/u/3521485?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Josep Servat</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=servatj" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/thuydx55"><img src="https://avatars2.githubusercontent.com/u/1469984?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Nick Dong</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=thuydx55" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/Stosiu"><img src="https://avatars1.githubusercontent.com/u/10252063?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Aleksander Stós</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=Stosiu" title="Code">💻</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/kdankert"><img src="https://avatars0.githubusercontent.com/u/46489624?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kjell Dankert</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=kdankert" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/juliendu11"><img src="https://avatars0.githubusercontent.com/u/18739442?v=4?s=100" width="100px;" alt=""/><br /><sub><b>juliendu11</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=juliendu11" title="Code">💻</a></td>
    <td align="center"><a href="https://me.io"><img src="https://avatars.githubusercontent.com/u/45731?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Mohamed Meabed</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=meabed" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/ofarukaydin"><img src="https://avatars.githubusercontent.com/u/32788963?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Faruk Aydın</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=ofarukaydin" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/dahlmo"><img src="https://avatars.githubusercontent.com/u/23076026?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Dahlmo</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=dahlmo" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/gandazgul"><img src="https://avatars.githubusercontent.com/u/108850?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Carlos Ravelo</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=gandazgul" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/paulish"><img src="https://avatars.githubusercontent.com/u/1762032?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Paul Ishenin</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=paulish" title="Code">💻</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/sbingner"><img src="https://avatars.githubusercontent.com/u/354533?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sam Bingner</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=sbingner" title="Code">💻</a></td>
    <td align="center"><a href="https://stackoverflow.com/users/5059657/alexander-staroselsky"><img src="https://avatars.githubusercontent.com/u/34102969?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alexander Staroselsky</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=alexstaroselsky" title="Code">💻</a></td>
    <td align="center"><a href="http://joelabrahamsson.com"><img src="https://avatars.githubusercontent.com/u/218986?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Joel Abrahamsson</b></sub></a><br /><a href="https://github.com/BRIKEV/express-jsdoc-swagger/commits?author=joelabrahamsson" title="Code">💻</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

Este proyecto sigue la especificación de [all-contributors](https://github.com/all-contributors/all-contributors). Las contribuciones de cualquier tipo son bienvenidas!
