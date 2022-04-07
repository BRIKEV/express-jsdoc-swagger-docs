# Segurança
Para adicionar [Autenticação](https://swagger.io/docs/specification/authentication/) nos seus endpoints com express-jsdoc-swagger, você deve adicionar seus esquemas de segurança ao objeto de configuração quando você cria a sua instância:

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

- A chave `security` é usada para definir seus esquemas de segurança, e é opcional caso você quiser nenhum tipo de segurança.

Após você adicionar seus esquemas de segurança, você pode usá-los em seus endpoints assim:

```javascript
/**
 * GET /api/v1
 * @summary Este é o resumo ou descrição do seu endpoint
 * @return {string} 200 - resposta de sucesso
 * @security BasicAuth
 */
app.get('/api/v1', (req, res) => res.send('Hello World!'));
```

O resultado na swagger UI será assim:

<img src="./assets/security.png"/>

## Múltiplos tipos de Autenticação

Você pode usar múltiplos tipos de autenticação e combinar os requisitos de segurança usando lógica OU (`|`) e E (`&`) para atingir o resultado desejado.

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
 * @summary Endpoint com múltiplas configurações de segurança (lógica E)
 * @return {string} 200 - resposta de sucesso
 * @security BasicAuth & BearerAuth
 */
app.get('/api/v2', (req, res) => res.send('Hello World!'));

/**
 * GET /api/v3
 * @summary Endpoint com múltiplas configurações de segurança (lógica OU)
 * @return {string} 200 - resposta de sucesso
 * @security BasicAuth | BearerAuth
 */
app.get('/api/v3', (req, res) => res.send('Hello World!'));
```

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/security/basic-auth.js).
