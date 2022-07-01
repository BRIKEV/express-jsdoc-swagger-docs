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
 * @summary Este é o resumo do endpoint
 * @return {object} 200 - resposta de sucesso
 */
app.get('/api/v1', (req, res) => res.json({
  success: true,
}));

app.listen(PORT, () => console.log(`Example app listening at http://localhost:${PORT}`));
```

## Exemplos
1. Configuração Básica

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
 * Um tipo de música
 * @typedef {object} Musica
 * @property {string} titulo.required - O título
 * @property {string} artista - O artista
 * @property {number} ano - O ano - double
 */
```

3. Endpoint que retorna um modelo de array de `Musicas`

```javascript
/**
 * GET /api/v1/albums
 * @summary Este é o resumo do endpoint
 * @tags album
 * @return {array<Musica>} 200 - resposta de sucesso - application/json
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    titulo: 'album 1',
  }])
));
```

1. Definição de endpoint básico com tags, parâmetros e autenticação básica

```javascript
/**
 * GET /api/v1/album
 * @summary Este é o resumo do endpoint
 * @security BasicAuth
 * @tags album
 * @param {string} nome.query.required - descrição do parâmetro name
 * @return {object} 200 - resposta de sucesso - application/json
 * @return {object} 400 - resposta Bad request
 */
app.get('/api/v1/album', (req, res) => (
  res.json({
    titulo: 'album 1',
  })
));
```

1. Definição de endpoint básico código de exemplo para o corpo da resposta

```javascript
/**
 * GET /api/v1/albums
 * @summary Este é o resumo do endpoint
 * @tags album
 * @return {array<Music>} 200 - resposta de sucesso - application/json
 * @example response - 200 - exemplo de resposta de sucesso
 * [
 *   {
 *     "titulo": "Bury the light",
 *     "artista": "Casey Edwards ft. Victor Borba",
 *     "ano": 2020
 *   }
 * ]
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'track 1',
  }])
));
```

Você pode encontrar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples).

## Validador

Nós desenvolvemos um novo pacote que funciona como um validador dos endpoints da sua API com a documentação que você cria com `express-jsdoc-swagger`. este pacote é [express-oas-validator](https://github.com/BRIKEV/express-oas-validator).

**Exemplo**

Instale usando o registro de pacote node:

```
npm install --save express-oas-validator
```

Ou usando yarn:

```
yarn add express-oas-validator
```

Após isso você deve inicializar usando o evento `finish`. Mais informações nestas [sessões](/pt/eventEmitter.md).

```js
const instance = expressJSDocSwagger(app)(options);

instance.on('finish', data => {
  init(data);
  resolve(app);
});
```

Este é o exemplo completo de como ele funciona.

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
   * Uma música
   * @typedef {object} Musica
   * @property {string} titulo.required - O título
   * @property {string} artista - O artista
   * @property {integer} ano - O ano
   */

  /**
   * POST /api/v1/songs
   * @param {Musica} request.body.required - informações da música
   * @return {object} 200 - resposta da música
   */
  app.post('/api/v1/songs', validateRequest(), (req, res) => res.send('Você tem uma música!'));

  /**
   * POST /api/v1/name
   * @param {string} request.body.required - descrição do corpo nome
   * @return {object} 200 - resposta da música
   */
  app.post('/api/v1/name', (req, res, next) => {
    try {
      // Valida a resposta
      validateResponse('Error string', req);
      return res.send('Hello World!');
    } catch (error) {
      return next(error);
    }
  });

  /**
   * GET /api/v1/authors
   * @summary este é o resumo ou descrição do endpoint
   * @param {string} nome.query.required - descrição do parâmetro nome - enum:type1,type2
   * @param {array<string>} licenca.query - descrição do parâmetro licença
   * @return {object} 200 - resposta de sucesso - application/json
   */
  app.get('/api/v1/authors', validateRequest({ headers: false }), (req, res) => (
    res.json([{
      titulo: 'album 1',
    }])
  ));

  // eslint-disable-next-line no-unused-vars
  app.use((err, req, res, next) => {
    res.status(err.status).json(err);
  });
});

module.exports = serverApp;
```

## Contribuintes ✨

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

Este projeto segue a especificação [all-contributors](https://github.com/all-contributors/all-contributors). Contribuições de todo tipo são bem vindas!
