# Tags
You can assign a list of [tags](https://swagger.io/docs/specification/grouping-operations-with-tags/) to each API operation.

To define a tag with express-jsdoc-swagger:
```javascript
/**
 * GET /api/v1/album
 * @tags Album - everything about album
 */
```
Where:
- The keyword `@tags` is used to assign to the endpoint.
- The following option, separated between ` - `, is the description.

Furthermore, the same tag can be assigned to several endpoints:
```javascript
/**
 * GET /api/v1/songs
 * @tags Album
 * @tags Songs - everything about songs
 */
```
In this case, `Album` and `Song` tags have been assigned to the songs' endpoint, and the result in swagger UI will be this:

<img src="./assets/tags.png"/>

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/tags).

