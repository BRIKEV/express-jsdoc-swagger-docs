# Configuration

To start using the package you need to add a configuration object. This object has the following options:

- [info](configuration.md?id=info)
- [servers](configuration.md?id=servers)
- [security](configuration.md?id=security)
- [filesPattern](configuration.md?id=filespattern)
- [baseDir](configuration.md?id=basedir)

## Info

The info options is the same as swagger specifies in [their documentation](https://swagger.io/specification/#info-object). It provides metadata about the API.

#### Info option example

```javascript
{
  "title": "Sample Pet Store App",
  "description": "This is a sample server for a pet store.",
  "termsOfService": "http://example.com/terms/",
  "contact": {
    "name": "API Support",
    "url": "http://www.example.com/support",
    "email": "support@example.com"
  },
  "license": {
    "name": "Apache 2.0",
    "url": "https://www.apache.org/licenses/LICENSE-2.0.html"
  },
  "version": "1.0.1"
}
```

## Servers

The servers options is the same as swagger specifies in [their documentation](https://swagger.io/specification/#server-object). An array of Server Objects, which provide connectivity information to a target server.

#### Server option example

```javascript
{
  "servers": [
    {
      "url": "https://development.gigantic-server.com/v1",
      "description": "Development server"
    },
    {
      "url": "https://staging.gigantic-server.com/v1",
      "description": "Staging server"
    },
    {
      "url": "https://api.gigantic-server.com/v1",
      "description": "Production server"
    }
  ]
}
```

## Security

The security options is the same as swagger specifies in [their documentation](https://swagger.io/specification/#security-requirement-object). A declaration of which security mechanisms can be used across the API. The list of values includes alternative security requirement objects that can be used.

#### Security option example

```javascript
{
  "security": {
    "BasicAuth": {
      "type": "http",
      "scheme": "basic"
    },
    "BearerAuth": {
      "type": "http",
      "scheme": "bearer"
    }
  }
}
```

## filesPattern

This required option you could add a path to one file or a [glob pattern](https://en.wikipedia.org/wiki/Glob_(programming)) to multiple files.

#### filesPAttern option example

```javascript
{
  "filesPattern": "./basic-auth.js"
}
```

```javascript
{
  "filesPattern": "./**/*-routes.js"
}
```

## baseDir

App absolute path.

#### baseDir option example

```javascript
{
  "baseDir": __dirname
}
```

## Full example

```javascript
const express = require('express');

const expressJSDocSwagger = require('express-jsdoc-swagger');

// This is a full set of options
// It is not neccesary to complete every option
const options = {
  info: {
    version: '1.0.0',
    title: 'Albums store',
    license: {
      name: 'MIT',
      url: 'http://example.com',
    },
    description: 'API desctiption',
    contact: {
      name: 'contact name',
      url: 'http://example.com',
      email: 'test@test.com',
    },
    termsOfService: 'http://example.com',
  },
  servers: [
    {
      url: 'https://{username}.gigantic-server.com:{port}/{basePath}',
      description: 'The production API server',
      variables: {
        username: {
          default: 'demo',
          description: 'this value is assigned by the service provider, in this example `gigantic-server.com`',
        },
        port: {
          enum: [
            '8443',
            '443',
          ],
          default: '8443',
        },
        basePath: {
          default: 'v2',
        },
      },
    },
  ],
  security: {
    BasicAuth: {
      type: 'http',
      scheme: 'basic',
    },
  },
  file: './main.js',
  baseDir: __dirname,
};

const app = express();
const port = 3000;

expressJSDocSwagger(app)(options);

/**
 * GET /api/v1
 * @summary This is the summary or description of the endpoint
 * @return {string} 200 - success response
 */
app.get('/api/v1', (req, res) => res.send('Hello World!'));

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`));
```



