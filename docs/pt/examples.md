# Exemplos

Para adicionar [exemplos](https://swagger.io/docs/specification/adding-examples/) nos seus endpoints com [express-jsdoc-swagger](https://github.com/BRIKEV/express-jsdoc-swagger), você pode usar a anotação `@example`.

O exemplo a seguir ilustra como pode ser usado para adicionar exemplos de como o corpo da requisição deve ser, assim como é esperado que o endpoind retorne por códigos de status específicos.

```javascript
/**
 * POST /api/v1/song
 * @param {Song} request.body.required - Song info
 * @return {object} 200 - Success response
 * @return {object} 400 - Bad request response
 * @example request - example payload
 * {
 *   "title": "Bury The Light",
 *   "artist": "Casey Edwards ft. Victor Borba",
 *   "year": 2020
 * }
 * @example request - other payload example
 * {
 *   "title": "The war we made",
 *   "artist": "Red",
 *   "year": 2020
 * }
 * @example response - 200 - example success response
 * {
 *   "message": "You have added a song!"
 * }
 * @example response - 400 - example error response
 * {
 *   "message": "Failed to save song because you did not specify a title",
 *   "errCode": "EV121"
 * }
 */
app.post('/api/v1/song', (req, res) =>
  res.send({
    message: 'You have added a song!',
  })
);
```

O resultado na Swagger UI será assim:

<img src="./assets/examples.png"/>

> Você pode encontrar mais exemplos funcionais [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/requestBody/withExamples.js) ou [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/responses/withExamples.js).

## Instruções de Uso

A anotação `@example` deve ser seguida imediatamente por uma palavra chave que vai representar o que nosso exemplo vai ilustrar: `request` ou `response`.

> É possível adicionar quantos exemplos forem necessários para ambas requisições e respostas em um único endpoint, nõ há limite para isso. No caso das respostas, múltiplos exemplos para o mesmo status code também são suportados (então nós podemos fornecer diferentes exemplos do que a nossa API vai retornar em caso de sucesso, por exemplo).

As sessões abaixo descrevem em mais detalhes a sintaxe esperada para a tag `@example` em cada caso (requisição e resposta). Embora seja o mesmo na maioria das vezes, haverão pequenas diferenças dependendo da palavra chave utilizada.

### Exemplo de Corpo da Requisição

```
@example request - [summary]
[content]
```

| Campo    | Descrição                                                                                                                                                                                                                                                                                     |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Summary | Pequeno texto que descreve brevemente o exemplo. Este texto deve ocupar apenas uma única linha. Tudo fora da linha da tab `@example` será considerado para do conteúdo do exemplo e _NÃO_ para o resumo.                                                                                       |
| Content | Exemplo do conteúdo do corpo da requisição. É obrigatório que seu conteúdo comece uma nov alinha abaixo da tag `@example`, e não na mesma linha. O conteúdo pode ser conter várias linhas se necessário, para melhor leitura. Indentação e quebras de linhas serão mantidos na Swagger UI. |

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/requestBody/withExamples.js).

### Exemplo de Corpo da Resposta

```
@example response - [status code] - [summary]
[content]
```

| Campo        | Descrição                                                                                                                                                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Status code | Um código de status An HTTP (ex.: `200`). tenha em mente que qualquer código nos [códigos de status válidos](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/transforms/paths/validStatusCodes.js) serão ignorados. Também, é necessário que haja uma tag `@return` para o mesmo código afim do exemplo aparecer na Swagger UI. |
| Summary     | Pequeno texto que descreve brevemente o exemplo. Este texto deve ocupar apenas uma única linha. Tudo fora da linha da tab `@example` será considerado para do conteúdo do exemplo e _NÃO_ para o resumo.                                                                                                                                    |
| Content     | Exemplo do conteúdo do corpo da requisição. É obrigatório que seu conteúdo comece uma nov alinha abaixo da tag `@example`, e não na mesma linha. O conteúdo pode ser conter várias linhas se necessário, para melhor leitura. Indentação e quebras de linhas serão mantidos na Swagger UI.                                             |

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/responses/withExamples.js).
