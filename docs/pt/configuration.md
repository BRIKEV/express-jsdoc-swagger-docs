# Configuração

Para começar usando o pacote você precisa para adicionar um objeto de configuração. Este objeto tem as seguintes opções:

- [info](/pt/configuration.md?id=info)
- [servers](/pt/configuration.md?id=servers)
- [security](/pt/configuration.md?id=security)
- [filesPattern](/pt/configuration.md?id=filespattern)
- [baseDir](/pt/configuration.md?id=basedir)

## Info

As opções de informação são as mesmas como o Swagger especifica na [documentação deles](https://swagger.io/specification/#info-object). Ela provê metadados sobre a API.

#### Exemplo da Opção Info

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

A opção dos servidores são as mesmas como o Swagger especifica na [documentação deles](https://swagger.io/specification/#server-object). Um array de objeto Server, que provê informações de conectividade para um servidor alvo.

#### Exemplo de opção Server

```javascript
{
  "servers": [
    {
      "url": "https://development.gigantic-server.com/v1",
      "description": "Servidor de Desenvolvimento"
    },
    {
      "url": "https://staging.gigantic-server.com/v1",
      "description": "Servidor de Testes"
    },
    {
      "url": "https://api.gigantic-server.com/v1",
      "description": "Servidor de Produção"
    }
  ]
}
```

## Security

As opções de segurança são as mesmas que o Swagger especifica na [documentação deles](https://swagger.io/specification/#security-requirement-object). Uma declaração de quais mecanismos de segurança podem ser usados na API. A lista de valores inclui objetos alternativos de requerimentos de segurança que podem ser usados.

#### Exemplo de Segurança

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

Esta opção obrigatória permite você adicionar um caminho para um arquivo ou um [padrão](<https://en.wikipedia.org/wiki/Glob_(programming)>) para múltiplos arquivos.

#### Exemplo de opção filesPattern

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

você também pode usar um array de arquivos

```javascript
{
  "filesPattern": ['./**/*.controller.js', './**/*.route.js']
}
```

## baseDir

O caminho absoluto da Aplicação.

#### Exemplo da opção baseDir

```javascript
{
  "baseDir": __dirname
}
```

## Exemplo completo

```javascript
const express = require('express');

const expressJSDocSwagger = require('express-jsdoc-swagger');

// Este é o exemplo completo de opções
// Não é necessário completar todas as opções
const options = {
  info: {
    version: '1.0.0',
    title: 'Loja de Albuns',
    license: {
      name: 'MIT',
      url: 'http://example.com',
    },
    description: 'Descrição da API',
    contact: {
      name: 'nome do contato',
      url: 'http://example.com',
      email: 'test@test.com',
    },
    termsOfService: 'http://example.com',
  },
  servers: [
    {
      url: 'https://{username}.gigantic-server.com:{port}/{basePath}',
      description: 'O servidor de produção da API',
      variables: {
        username: {
          default: 'demo',
          description:
            'Este valor é associado ao provedor do serviço, neste exemplo é `gigantic-server.com`',
        },
        port: {
          enum: ['8443', '443'],
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
  filesPattern: './main.js',
  baseDir: __dirname,
  // URL onde o SwaggerUI será renderizado
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

const app = express();
const port = 3000;

expressJSDocSwagger(app)(options);

/**
 * GET /api/v1
 * @summary Este é o resumo ou descrição do endpoint
 * @return {string} 200 - resposta com sucesso
 */
app.get('/api/v1', (req, res) => res.send('Hello World!'));

app.listen(port, () =>
  console.log(`Example app listening at http://localhost:${port}`)
);
```
