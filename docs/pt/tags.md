# Etiquetas
Você pode associar uma lista de [etiquetas](https://swagger.io/docs/specification/grouping-operations-with-tags/) para cada operação da API.

Para definir uma etiqueta com express-jsdoc-swagger:
```javascript
/**
 * GET /api/v1/album
 * @tags Album - tudo sobre álbum
 */
```
Onde:
- A palavra chave `@tags` é usada para associar etiquetas ao endepoint.
- a opção seguinte, separada entre ` - `, é a descrição.

Furthermore, the same tag can be assigned to several endpoints:
```javascript
/**
 * GET /api/v1/songs
 * @tags Album
 * @tags Songs - everything about songs
 */
```
Neste caso, as etiquetas `Album` e `Song` foram associadas ao endepoint de músicas (Songs), e o resultado na swagger UI será este:

<img src="./assets/tags.png"/>

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/tags).

