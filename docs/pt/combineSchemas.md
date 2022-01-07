# Combinar esquemas

Para combinar esquemas como você faria no [Swagger](https://swagger.io/docs/specification/data-models/oneof-anyof-allof-not/) você pode adicionar estes comentários:

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

No exemplo acima, você pode ver que estamos usando `oneOf` e dois esquemas que nós queremos usar como resposta para nosso endpoint.

É **importante** que o critério (`oneOf`, `anyOf` e `allOf`) deve ser o primeiro valor se seguido por `|` e cada esquema, assim:

```
{<criterio>|<esquema>|...|<esquemaN>}

{oneOf|MusicaInstrumental|MusicaPop}

// Ou
{MusicaInstrumental|MusicaPop}
// um destes é o valor padrão
```

Atualmente, nós suportamos `oneOf`, `anyOf` e `allOf`. O valor padrão é `oneOf`.

> Você pode extender nosso suporte ao Open API extendendo seu próprio arquivo swagger.json, por favor visite [Sessão Extenda seu arquivo swagger](/pt/merge.md) para mais informações.

> Para aprender como definir esquemas de componentes, por favor visite a sessão [componentes / esquemas](/pt/components.md).

> Você pode encontrar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/combineesquemas/index.js).
