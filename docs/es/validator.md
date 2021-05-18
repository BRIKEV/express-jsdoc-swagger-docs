# express-oas-validator

Puedes añadir validación a los parámetros request params, headers, body y a la response con [express-oas-validator](https://github.com/BRIKEV/express-oas-validator). Un módulo simple que exporta unos métodos de validación y un middleware de express para que puedas validar los parametros de tu request con la documentación que generas en tu API.

## Quick start
Instalar express-oas-validator con npm:

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

Ejemplo completo de como integrarlo con `express-jsdoc-swagger`

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

## express-oas-validator methods

### init(openApiDef, options)

Este método inicializa el validador para poder usar `validateRequest` y `validateResponse`.

**Parameters**

| Name        | Type   | Description        |
| ------------|:------:| ------------------:|
| openApiDef  | object | OpenAPI definición |
| options     | object | Options para extender la config de Ajv |

```js
const swaggerDefinition = require('./swaggerDefinition.json');

init(swaggerDefinition);
```


## validateRequest(endpointConfig)

El middleware recibe esta configuración.

```js
const DEFAULT_CONFIG = {
  body: true,
  params: true,
  headers: true,
  query: true,
  required: true,
  errorStatusCode: 400,
};
```

**Example**

```js
// This one uses the DEFAULT_CONFIG
app.get('/api/v1/albums/:id', validateRequest(), (req, res) => (
  res.json([{
    title: 'abum 1',
  }])
));

// With custom configuration
app.get('/api/v1/albums/:id', validateRequest({ headers: false }), (req, res) => (
  res.json([{
    title: 'abum 1',
  }])
));
```

## validateResponse(payload, req, status)

Método para validad la respuesta de una petición con la documentación generada.

**Parameters**

| Name        | Type   | Description        |
| ------------|:------:| ------------------:|
| payload     | *      | Respuesta a validar |
| req         | object | Options para extender la config de Ajv |
| status      | number | estatus de la respuesta que queremos validar |


**Example**

```js
validateResponse('Error string', req, 200);
```
