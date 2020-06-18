# Event emitter

We expose an [event emitter](https://nodejs.org/api/events.html) in case you want to know more about what is happening while we parse the comments or the Swagger object result.

These are the events we expose:

- **error** you can subscribe to this event to debug if there is a problem while the parsing process is running.
- **process** you can subscribe to this event to debug how jsdoc comments are being parsed into swagger object per entity.
- **finish** you can subscribe to receive the result of the swagger object generated.

## Example

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

// Event emitter API
listener.on('error', error => {
  console.error(`Error: ${error}`);
});

listener.on('process', ({ entity, swaggerObject }) => {
  console.log(`entity: ${entity}`);
  console.log('swaggerObject');
  console.log(swaggerObject);
});

listener.on('finish', swaggerObject => {
  console.log('Finish');
  console.log(swaggerObject);
});

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`));
````

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/eventEmitter).

