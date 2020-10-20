# Responses
Para añadir [responses](https://swagger.io/docs/specification/describing-responses/) a tus endpoints con `express-jsdoc-swagger`, puedes añadir los siguientes comentarios:

```javascript
/**
 * GET /api/v1
 * @summary This is the summary or description of the endpoint
 * @return {string} 200 - success response
 * @return {object} 400 - Bad request response
 */
app.get('/api/v1', (req, res) => res.send('Hello World!'));
```

Donde:
- `@summary` es la descipción del endpoint.
- `@return` define la respuesta.
- El [tipo](https://swagger.io/specification/#data-types) de define entre `{}`.
- Después del tipo, se define el código de estado HTTP.
- La siguiente opción, separada entre ` - `, es la descripción.

Puede marcar un endpoint como deprecado con `@deprecated`:
```javascript
/**
 * GET /api/v1/album
 * @summary This is the summary or description of the endpoint
 * @deprecated
 * @return {object} 200 - success response
 * @return {object} 400 - Bad request response
 */
app.get('/api/v1/album', (req, res) => (
  res.json({
    title: 'abum 1',
  })
));
```
Y el resultado será el siguiente:

<img src="./assets/deprecated.png"/>

También se puede asignar una serie de `tags` a cada endpoint:
```javascript
/**
 * GET /api/v2/album
 * @summary This is the summary or description of the endpoint
 * @tags album
 * @security BasicAuth
 * @return {object} 200 - success response
 * @return {object} 400 - Bad request response
 */
app.get('/api/v2/album', (req, res) => (
  res.json({
    title: 'abum 1',
  })
));
```
> Si quieres saber más acerda de los `@tags`, por favor visita la sección [tags](tags.md).

Se puede retornar un esquema *(Song)*:
```javascript
/**
 * GET /api/v1/albums
 * @summary This is the summary or description of the endpoint
 * @tags album
 * @return {array<Song>} 200 - success response - application/json
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'abum 1',
  }])
));
```
En este caso:
- El endpoint devuelve un array de caciones.
- La última opción del `@return` *(application/json)* específica los tipos de respuesta. Esto es opcional y su valor por defecto es *application/json*.

El swagger resultante será:

<img src="./assets/response-component.png"/>

> Para aprender a añadir ejemplos de la salida de tu endpoint, mira la sección [examples](examples.md).

> Para aprender cómo definir los componentes, por favor visite la sección [components](components.md).

> Puedes ver más ejemplos [aquí](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/responses).
