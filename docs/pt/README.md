![npm](https://img.shields.io/npm/v/express-jsdoc-swagger)
![Node.js Package](https://github.com/BRIKEV/express-jsdoc-swagger/workflows/Build/badge.svg)
[![Known Vulnerabilities](https://snyk.io/test/github/BRIKEV/express-jsdoc-swagger/badge.svg)](https://snyk.io/test/github/BRIKEV/express-jsdoc-swagger)
[![Maintainability](https://api.codeclimate.com/v1/badges/6d5565df0c9c10e75b59/maintainability)](https://codeclimate.com/github/BRIKEV/express-jsdoc-swagger/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/6d5565df0c9c10e75b59/test_coverage)](https://codeclimate.com/github/BRIKEV/express-jsdoc-swagger/test_coverage)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
![npm](https://img.shields.io/npm/dm/express-jsdoc-swagger)

# express-jsdoc-swagger

Com esta biblioteca, você pode documentar seus endpoints usando Swagger [Especificação OpenAPI 3](https://swagger.io/specification/) sem escrever aquivos YAML ou JSON. Você pode escrever comentários jsdoc em cada endpoint, e a biblioteca vai criar a Swagger UI.

## Pré Requisitos
Esta biblioteca assume que você está usando:
1. [NodeJS](https://nodejs.org)
2. [Express.js](http://www.expressjs.com)

## Instalação
```
npm i express-jsdoc-swagger
```

ou usando yarn

```
yarn add express-jsdoc-swagger
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
  // Padrão para encontrar seus arquivos jsdoc files (múltiplos padrões podem ser adicionados em um array)
  filesPattern: './**/*.js',
  // URL onde a SwaggerUI será renderizada
  swaggerUIPath: '/api-docs',
  // Expõe OpenAPI UI
  exposeSwaggerUI: true,
  // Expõe a documentação Open API JSON Docs no caminho `apiDocsPath`.
  exposeApiDocs: false,
  // Caminho do Endpoint Open API JSON Docs.
  apiDocsPath: '/v3/api-docs',
  // Define os campos não obrigatórios como nullable por padrão
  notRequiredAsNullable: false,
  // Você pode customizar suas opções de UI.
  // Você pode estender a configuração do swagger-ui-express. Você pode ver um exemplo destes
  // no `example/configuration/swaggerOptions.js`
  swaggerUiOptions: {},
};

const app = express();
const PORT = 3000;

expressJSDocSwagger(app)(options);

/**
 * GET /api/v1
 * @summary This is the summary of the endpoint
 * @return {object} 200 - success response
 */
app.get('/api/v1', (req, res) => res.json({
  success: true,
}));

app.listen(PORT, () => console.log(`Example app listening at http://localhost:${PORT}`));
```

## Examples
1. Basic configuration

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
  baseDir: __dirname,
  // URL onde a SwaggerUI será renderizado
  swaggerUIPath: '/api-docs',
  // Expõe OpenAPI UI
  exposeSwaggerUI: true,
  // Expõe a documentação Open API JSON Docs no caminho `apiDocsPath`.
  exposeApiDocs: false,
  // Caminho do Endpoint Open API JSON Docs.
  apiDocsPath: '/v3/api-docs',
  // Define os campos não obrigatórios como nullable por padrão
  notRequiredAsNullable: false,
};
```

2. Definição de componentes

```javascript
/**
 * A song type
 * @typedef {object} Song
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {number} year - The year - double
 */
```

3. Endpoint que retorna um modelo de array de `Songs`

```javascript
/**
 * GET /api/v1/albums
 * @summary This is the summary of the endpoint
 * @tags album
 * @return {array<Song>} 200 - success response - application/json
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'album 1',
  }])
));
```

4. Basic endpoint definition with tags, params and basic authentication

```javascript
/**
 * GET /api/v1/album
 * @summary This is the summary of the endpoint
 * @security BasicAuth
 * @tags album
 * @param {string} name.query.required - name param description
 * @return {object} 200 - success response - application/json
 * @return {object} 400 - Bad request response
 */
app.get('/api/v1/album', (req, res) => (
  res.json({
    title: 'album 1',
  })
));
```

5. Basic endpoint definition with code example for response body

```javascript
/**
 * GET /api/v1/albums
 * @summary This is the summary of the endpoint
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

You can find more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples).

## Validator

We developed a new package that works as a validator of your API endpoints with the documentation you create with `express-jsdoc-swagger`. This package is [express-oas-validator](https://github.com/BRIKEV/express-oas-validator).

**Example**

Install using the node package registry:

```
npm install --save express-oas-validator
```

After this you have to initialize using the `finish` event. More info in this [sections](eventEmitter.md).

```js
const instance = expressJSDocSwagger(app)(options);

instance.on('finish', data => {
  init(data);
  resolve(app);
});
```

This is a full example of how it works.

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
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
