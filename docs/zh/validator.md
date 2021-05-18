# express-oas-validator

You can add validation to your request params, headers, body and response with [express-oas-validator](https://github.com/BRIKEV/express-oas-validator) a simple module that export some validation methods and an express middleware to unify the validation your API provides with the Documentation you provide. 

## Quick start
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

This is a full example on how it works.

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

This methods initiates the validator so that `validateRequest` and `validateResponse` can be used in different files.

**Parameters**

| Name        | Type   | Description        |
| ------------|:------:| ------------------:|
| openApiDef  | object | OpenAPI definition |
| options     | object | Options to extend the errorHandler or Ajv configuration |

```js
const swaggerDefinition = require('./swaggerDefinition.json');

init(swaggerDefinition);
```


## validateRequest(endpointConfig)


Express middleware that receives this configuration options and validates each of the options.

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

Method to validate response payload based on the docs and the status we want to validate.

**Parameters**

| Name        | Type   | Description        |
| ------------|:------:| ------------------:|
| payload     | *      | response we want to validate |
| req         | object | Options to extend the errorHandler or Ajv configuration |
| status      | number | esponse status we want to validate |


**Example**

```js
validateResponse('Error string', req, 200);
```
