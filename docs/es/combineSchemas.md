# Combinar esquemas
Para combinar los esquemas, al igual que se puede hacer en [Swagger](https://swagger.io/docs/specification/data-models/oneof-anyof-allof-not/), con `express-jsdoc-swagger` se haría lo siguiente:

```javascript
/**
 * A song
 * @typedef {object} IntrumentalSong
 * @property {string} title.required - The title
 * @property {string} band - The band
 * @property {number} year - The year - double
 */

/**
 * A song
 * @typedef {object} PopSong
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {integer} year - The year - int64
 */

/**
 * GET /api/v1
 * @return {oneOf|IntrumentalSong|PopSong} 200 - success response - application/json
 */
```
En el ejemplo anterior, se puede ver que se ha usado `oneOf` y a continuación se han definido los dos esquemas *(Song y PopSong)* que queremos usar como respuesta de nuestro endpoint

Es **important** que la palabra reservada `oneOf` sea el primer valor y que esté seguido de `|` y el esquema, como se puede ver a continuación:
```
{<criteria>|<schema>|...|<schemaN>}

{oneOf|IntrumentalSong|PopSong}
```

Actualmente la librería soporta las opciones, `oneOf`, `anyOf` y `allOf`.

> Se puede ampliar las funcionalidades que express-jsdoc-swagger a un soporte completo de Open API, extendiendo tu propio swagger.json, para saber más visita la sección [Extender tu archivo de swagger](merge.md).

> Para aprender cómo definir los esquemas de los componentes, por favor visite la sección [Componentes / esquemas](components.md).

> Puedes ver más ejemplos [aquí](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/combineSchemas/index.js).
