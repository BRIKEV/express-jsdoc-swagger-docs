# Respostas
Para adicionar [respostas](https://swagger.io/docs/specification/describing-responses/) aos seus endpoints com express-jsdoc-swagger, você pode adicionar estes comentários:

```javascript
/**
 * GET /api/v1
 * @summary Este é o resumo ou descrição do endpoint
 * @return {string} 200 - resposta de sucesso
 * @return {object} 400 - resposta de má requisição (Bad request)
 */
app.get('/api/v1', (req, res) => res.send('Hello World!'));
```

Where:
- `@summary` é adescrição do endpoint.
- `@return` é usado para definir a resposta.
- [Tipo](https://swagger.io/specification/#data-types) está definido entre `{}`.
- Após o tipo, você deve definir o código de status HTTP.
- A opção seguinte, separada entre ` - `, é a descrição.

Você pode marcar operações específicas como depreciadas com a palavra chave`@deprecated`:
```javascript
/**
 * GET /api/v1/album
 * @summary Este é o resumo ou descrição do endpoint
 * @deprecated
 * @return {string} 200 - resposta de sucesso
 * @return {object} 400 - resposta de má requisição (Bad request)
 */
app.get('/api/v1/album', (req, res) => (
  res.json({
    title: 'abum 1',
  })
));
```
Fica assim:

<img src="./assets/deprecated.png"/>

Você também pode definir uma lista de tags para cada operação da API:
```javascript
/**
 * GET /api/v2/album
 * @summary Este é o resumo ou descrição do endpoint
 * @tags album
 * @security BasicAuth
 * @return {string} 200 - resposta de sucesso
 * @return {object} 400 - resposta de má requisição (Bad request)
 */
app.get('/api/v2/album', (req, res) => (
  res.json({
    title: 'abum 1',
  })
));
```
> Se você quiser saber mais sobre `@tags`, por favor visite a sessão de [tags](/pt/tags.md).

Você pode retornar um esquema comum:
```javascript
/**
 * GET /api/v1/albums
 * @summary Este é o resumo ou descrição do endpoint
 * @tags album
 * @return {array<Song>} 200 - resposta de sucesso - application/json
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'abum 1',
  }])
));
```
Neste caso:
- O endpoint retorna um array de Song.
- A última opção da palavra chave `@return` *(application/json)* especifica o tipo de mídia da resposta. Este é opção e seu valor padrão é *application/json*.

O resultado na swagger UI será este:

<img src="./assets/response-component.png"/>

> Para aprender como adicionar exemplos da saída do seu endpoint, veja a sessão de [exemplos](/pt/examples.md).

> Para aprender como definir esquemas de componentes, por favor visite a sessão de [componentes](/pt/components.md).

> Você pode ver mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/responses).
