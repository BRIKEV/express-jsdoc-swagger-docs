# express-oas-validator

Você pode adicionar validação aos seus parâmetros de requisição, cabeçalhos, corpo e resposta com [express-oas-validator](https://github.com/BRIKEV/express-oas-validator) um módulo simples que expota alguns métodos de validação e um middleware express para unificar a validação com a Documentação que vocẽ fornece para sua API.

## Início rápido

Instale usando o gerenciador de pacotes node:

```
npm install --save express-oas-validator
```

ou yarn:

```
yarn add express-oas-validator
```

Após isso, você deve inicializar usando o evento `finish`. Para mais informações, veja esta [sessão](/pt/eventEmitter.md).

```js
const instance = expressJSDocSwagger(app)(options);

instance.on('finish', data => {
  init(data);
  resolve(app);
});
```

este é um exemplo completo de como funciona.

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
   * @param {Song} request.body.required - infomrações da música
   * @return {object} 200 - resposta de música
   */
  app.post('/api/v1/songs', validateRequest(), (req, res) => res.send('Você salvou uma música!'));

  /**
   * POST /api/v1/name
   * @param {string} request.body.required - descrição do corpo da requisição
   * @return {object} 200 - resposta de música
   */
  app.post('/api/v1/name', (req, res, next) => {
    try {
      // Valida a resposta
      validateResponse('Mensagem de erro', req);
      return res.send('Olá Mundo!');
    } catch (error) {
      return next(error);
    }
  });

  /**
   * GET /api/v1/authors
   * @summary este é o resumo ou descrição do endpoint
   * @param {string} nome.query.required - descrição do parâmetro nome - enum:tipo1,tipo2
   * @param {array<string>} licenca.query - descrição do parâmetro licença
   * @return {object} 200 - resposta de sucesso - application/json
   */
  app.get('/api/v1/authors', validateRequest({ headers: false }), (req, res) => (
    res.json([{
      titulo: 'abum 1',
    }])
  ));

  // eslint-disable-next-line no-unused-vars
  app.use((err, req, res, next) => {
    res.status(err.status).json(err);
  });
});

module.exports = serverApp;
```

## métodos do express-oas-validator

### init(openApiDef, options)

Este método inicializa o validador então os métodos `validateRequest` e `validateResponse` podem ser usados em arquivos diferentes.

**Parâmetros**

| Nome        | Tipo   | Descrição          |
| ------------|:------:| ------------------:|
| openApiDef  | object | Definição OpenAPI  |
| options     | object | Opções para extender o gerenciador de erros (errorHandler) ou qualquer configuração |

```js
const swaggerDefinition = require('./swaggerDefinition.json');

init(swaggerDefinition);
```


## validateRequest(endpointConfig)


Middleware Express que recebe estas opções de configuração e valida cada uma das opções.

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

**Exemplo**

```js
// Este usa a DEFAULT_CONFIG
app.get('/api/v1/albums/:id', validateRequest(), (req, res) => (
  res.json([{
    titulo: 'abum 1',
  }])
));

// Com configuração personalizada
app.get('/api/v1/albums/:id', validateRequest({ headers: false }), (req, res) => (
  res.json([{
    titulo: 'abum 1',
  }])
));
```

## validateResponse(payload, req, status)

Método para validar os dados da resposta baseado na documentação e status que queremos validar.

**Parâmetros**

| Nome        | Tipo   | Descrição          |
| ------------|:------:| ------------------:|
| payload     | *      | resposta que queremos validar |
| req         | object | Opções para extender o gerenciador de erros (errorHandler) ou qualquer configuração |
| status      | number | status de resposta que queremos validar |


**Exemplo**

```js
validateResponse('Error string', req, 200);
```
