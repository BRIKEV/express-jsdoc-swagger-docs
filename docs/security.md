# Security
To add [Authentication](https://swagger.io/docs/specification/authentication/) to your endpoints with express-jsdoc-swagger, you have to add your security schemas to the config object when you create your instance:

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

- `Security` key is used to define your security schemas, and it is optional in case you don't want any type of security.

After you add your security schemas, you can use them in your endpoints like this:

```javascript
/**
 * GET /api/v1
 * @summary This is the summary or description of the endpoint
 * @return {string} 200 - success response
 * @security BasicAuth
 */
app.get('/api/v1', (req, res) => res.send('Hello World!'));
```

The result in swagger UI will be this:

<img src="./assets/security.png"/>

## Multiple Authentication types

You can use multiple authentication types and combine the security requirements using logical OR (`|`) and AND (`&`) to achieve the desired result.

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


> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/security/basic-auth.js).
