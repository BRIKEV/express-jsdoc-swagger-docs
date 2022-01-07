# Combinar Esquemas

Para combinar esquemas como você faria no [Swagger](https://swagger.io/docs/specification/data-models/oneof-anyof-allof-not/) você pode adicionar estes comentários:

```javascript
/**
 * Uma música
 * @typedef {object} MusicaInstrumental
 * @property {string} titulo.required - O título
 * @property {string} banda - A banda
 * @property {number} ano - O ano - double
 */

/**
 * Uma música
 * @typedef {object} MusicaPop
 * @property {string} titulo.required - The titulo
 * @property {string} artista - O artista
 * @property {integer} ano - O ano - int64
 */

/**
 * GET /api/v1
 * @return {oneOf|MusicaInstrumental|MusicaPop} 200 - resposta com sucesso - application/json
 */
```

No exemplo acima, você pode ver que nós usamos `oneOf` e dois esquemas que nós usamos como resposta para nosso endpoint.

É **importante** que os critérios (`oneOf`, `anyOf` and `allOf`) deve ser o primeiro valor seguido de `|` e cada esquema como no exemplo a seguir:

```
{<criterio>|<esquema>|...|<esquemaN>}

{oneOf|MusicaInstrumental|MusicaPop}

// Ou
{MusicaInstrumental|MusicaPop}
// Um deles é o valor padrão
```

Atualmemnte, nós suportamos `oneOf`, `anyOf` e `allOf`. O valor padrão é `oneOf`.

> Você pode extender o suporte ao Open API modificando seu próprio arquivo swagger.json, por favor visite [Sessão Extenda seu arquivo swagger](/pt/merge.md) para mais informações.

> Para aprender como definir esquemas de componentes, por favor visite a sessão [componentes / esquemas](/pt/components.md).

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/combineSchemas/index.js).
