# Configuración

Para empezar a utilizar el paquete, hay que añadir un objeto de configuración. Este objeto tiene las siguientes opciones:

- [info](configuration.md?id=info)
- [servers](configuration.md?id=servers)
- [security](configuration.md?id=security)
- [filesPattern](configuration.md?id=filespattern)
- [baseDir](configuration.md?id=basedir)

## Info

Las opciones de `info` son las mismas que swagger especifica en [su documentación](https://swagger.io/specification/#info-object) y proporciona metadatos sobre el API.

#### Ejemplo

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

Las opciones de `servers` son las mismas que swagger especifica en [su documentación](https://swagger.io/specification/#server-object). Se trata de un `Array` de objetos que proporciona información de conectividad a a un servidor destino.

#### Ejemplo

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

Las opciones de `security` son las mismas que swagger especifica en [su documentación](https://swagger.io/specification/#security-requirement-object). Consiste la configuración de los mecanismos de seguridad que pueden utilizarse en la API. Esto incluye objetos, que se corresponden a los distintas opciones de seguridad que se podrán utilizar.

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

Esta opción, la cual es requerida, podría añadir una ruta a un archivo o a un [patrón global](https://en.wikipedia.org/wiki/Glob_(programming)) de varios archivos.

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

you could also use array of files

```javascript
{
  "filesPattern": ['./**/*.controller.js', './**/*.route.js']
}
```

## baseDir

Ruta absoluta de la app

#### Ejemplo

```javascript
{
  "baseDir": __dirname
}
```

## Ejemplo completo

```javascript
const express = require('express');

const expressJSDocSwagger = require('express-jsdoc-swagger');

// Este es un ejemplo completo
// No es necesario completar todas las opciones
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



