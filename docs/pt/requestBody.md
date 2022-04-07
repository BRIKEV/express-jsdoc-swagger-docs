# Corpo da requisição
Para adicionar [informações do corpo](https://swagger.io/docs/specification/describing-request-body/) para seus endpoins com express-jsdoc-swagger, você pode adicionar estes comentários:

```javascript
/**
 * POST /api/v1/album
 * @param {object} request.body.required - songs info - application/json
 */
app.post('/api/v1/album', (req, res) => res.send('You save a song!'));
```

Onde:
- `@param` é usado para definir um parâmetro.
- [Tipo](https://swagger.io/specification/#data-types) é deifinido entre `{}`.
- Após o tipo, vocẽ deve definir uma chave **request.body**. esta chave deve ter um valor que você quer como informação do corpo.
- A opção seguinte, separado entre ` - `, é a descrição.
- A última opção da palavra chave `@param` *(application/json)* especifica o tipo de mídia. Este é opcional e seu valor padrão é *application/json*.

Você pode combinar isto com [componentes](components.md) para definir as informações baseado no componente como neste exemplo:

```javascript
/**
 * A song
 * @typedef {object} Song
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {integer} year - The year - int64
 */

/**
 * POST /api/v1/song
 * @param {Song} request.body.required - song info
 * @return {object} 200 - song response
 */
app.post('/api/v1/songs', (req, res) => res.send('You save a song!'));

/**
 * POST /api/v1/album
 * @param {array<Song>} request.body.required - songs info
 * @return {object} 200 - album response
 */
app.post('/api/v1/album', (req, res) => res.send('You save a song!'));
````

Ou você pode adicionar requisições de dados de formulário para upload de arquivos:

```javascript
/**
 * A song
 * @typedef {object} Song
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {string} cover - image cover - binary
 * @property {integer} year - The year - int64
 */

/**
 * POST /api/v1/album
 * @param {Song} request.body.required - songs info - multipart/form-data
 * @return {object} 200 - Album created
 */
app.post('/api/v1/album', (req, res) => res.send('You save a song!'));
```

Neste exemplo, nos usamos a última opção da palavra chave `@param` para especificar o tipo de mídia da requisição como multipart/form-data.

Na Swagger UI o resultado será assim:

<img src="./assets/request-body.png"/>

> Para aprender como adicionar exemplos das informações dos seus endpoints, veja a sessão de [exemplos](/pt/examples.md).

> Para aprender como definir esquemas de componentes, por favor visite a sessão de [componentes](/pt/components.md).

> Você pode encontrar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/requestBody).

### Corpo como um parâmetro de formulário

Você pode enviar parâmetros de corpo sem usar um componente. Para fazer isso você pode adicionar estes comentários:

```javascript
/**
 * POST /api/v1/song
 * @param {string} id.form.required - This is the song id - application/x-www-form-urlencoded
 * @param {string} title.form.required - This is the song title - application/x-www-form-urlencoded
 * @return {object} 200 - song response
 */
app.post('/api/v1/songs', (req, res) => res.json({}));
```

Onde:
- `@param` é usado para definir um parâmetro.
- [Tipo](https://swagger.io/specification/#data-types) é definido entre `{}`.
- Após o tipo, você deve definir a chave que você deseja para o parâmetro seguido do valor **form**.
- A opção seguinte, separado entre ` - `, é a descrição.
- a última opção de palavra chave `@param` é *(application/json)* que especifica o tipo de mídia. Este valor é opcional e seu valor padrão é *application/json*.

**IMPORTANTE:** Para usar os parâmetros de formulário é necessário fornecer uma descrição para cada campo.

> Você pode encontrar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/requestBody/formParameters.js).
