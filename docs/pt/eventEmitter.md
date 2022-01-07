# Emissor de Eventos

Nós expomos um [emissor de eventos](https://nodejs.org/api/events.html) caso você quiser saber mais sobre o que está acontecendo enquando nós processamos os comentários ou o resultado do objeto Swagger.

Estes são os eventos que nós expomos:

- **error** você pode se inscrever neste evento para depurar se há algum problema enquanto o processo de analise está executando.
- **process** você pode se inscrever neste evento para depurar como os comentários jsdoc stão sendo analisados dentro do objeto swagger por entidade.
- **finish** você pode se inscrever neste evento para receber o objeto swagger de resultado que foi gerado e alguns métodos que nós usamos para processar o objeto swagger em caso você queira usar em seu código.

## Exemplo

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
  filesPattern: './simple.js',
  baseDir: __dirname,
};

const app = express();
const port = 3000;

const listener = expressJSDocSwagger(app)(options);

// API Emissão de Eventos
listener.on('error', (error) => {
  console.error(`Error: ${error}`);
});

listener.on('process', ({ entity, swaggerObject }) => {
  console.log(`entity: ${entity}`);
  console.log('swaggerObject');
  console.log(swaggerObject);
});

listener.on('finish', (swaggerObject, processMethods) => {
  console.log('Finish');
  console.log(swaggerObject, processMethods);
});

app.listen(port, () =>
  console.log(`Example app listening at http://localhost:${port}`)
);
```

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/eventEmitter).
