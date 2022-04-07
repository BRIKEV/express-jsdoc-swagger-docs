# Parâmetros
para adicionar [parâmetros](https://swagger.io/docs/specification/describing-parameters/) aos seus endpoints com express-jsdoc-swagger, você pode adicionar estes comentários:

```javascript
/**
 * GET /api/v1/{id}
 * @summary Este é o resumo ou descrição do endpoint
 * @param {string} name.query.required - descrição do parâmetro name
 * @param {number} id.path - número do telefone
 * @return {string} 200 - resposta de sucesso
 */
app.get('/api/v1/:id', (req, res) => res.send('Hello World!'));
````

Onde:
- `@param` é usado para definir um parâmetro.
- [Tipo](https://swagger.io/specification/#data-types) é definido entre chaves `{}`.
- Após o tipo, você pode definir a chave que você desejar para o parâmetro.
- Você pode definir o status do seu parâmetro como estes:
  - `* @param {string} name.query.required` name será um parâmetro de consulta (query param) obrigatório.
  - `* @param {string} name.query.deprecated` name será um parâmetro de consulta (query param) depreciado.
  - `* @param {string} name.query` name será um parâmetro de consulta (query param).
- A opção seguinte, separada por ` - `, é a descrição.

Você pode adicionar **valores de enum** aos seus parâmetros assim:

```javascript
/**
 * GET /api/v1/albums
 * @summary Este é o resumo ou descrição do endpoint
 * @param {string} name.query.required - descrição do parâmetro name - enum:type1,type2
 * @param {string} license.query - enum:MIT,ISC - descrição do parâmetro license
 * @return {object} 200 - resposta de sucesso - application/json
 */
```

O últimos parâmetro pode ser usado como um enum, ou você pode trocar entre enum e descrição.

O resultado na Swagger UI será este:

<img src="./assets/parameters.png"/>

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/parameters).
