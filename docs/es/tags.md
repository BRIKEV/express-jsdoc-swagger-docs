# Tags
Se puede asignar un lista de [tags](https://swagger.io/docs/specification/grouping-operations-with-tags/) a cada endpoint.

Con `express-jsdoc-swagger` los tags se definen así:
```javascript
/**
 * GET /api/v1/album
 * @tags Album - everything about album
 */
```
Donde:
- `@tags` se usa para asignar un tag al endpoint.
- La siguiente opción, separada entre ` - `, es la descipción.

Además, el mismo tag se puede asignar a varios endpoints:
```javascript
/**
 * GET /api/v1/songs
 * @tags Album
 * @tags Songs - everything about songs
 */
```
En el caso anterior, los tags `Album` y `Song` se han asignado al endpoint `Songs`,y el swagger resultante será:

<img src="./assets/tags.png"/>

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/tags).

