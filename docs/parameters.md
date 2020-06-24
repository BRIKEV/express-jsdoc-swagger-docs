# Parameters
To add [parameters](https://swagger.io/docs/specification/describing-parameters/) to your endpoints with express-jsdoc-swagger, you could add these comments:

```javascript
/**
 * GET /api/v1/{id}
 * @summary This is the summary or description of the endpoint
 * @param {string} name.query.required - name param description
 * @param {number} id.path - phone number
 * @return {string} 200 - success response
 */
app.get('/api/v1/:id', (req, res) => res.send('Hello World!'));
````

Where:
- `@param` is used to define one parameter.
- [Type](https://swagger.io/specification/#data-types) is defined between `{}`.
- After the type, you have to define the key you want for the parameter.
- You can define the status of your param like this:
  - `* @param {string} name.query.required` name will be a required query param.
  - `* @param {string} name.query.deprecated` name will be a deprecated query param.
  - `* @param {string} name.query` name will be a query param.
- The following option, separated between ` - `, is the description.

You can add **enum values** to your parameters like this:

```javascript
/**
 * GET /api/v1/albums
 * @summary This is the summary or description of the endpoint
 * @param {string} name.query.required - name param description - enum:type1,type2
 * @param {string} license.query - enum:MIT,ISC - name param description
 * @return {object} 200 - success response - application/json
 */
```

The last parameter can be used as an enum, or you can switch between enum and description.

The result in swagger UI will be this:

<img src="./assets/parameters.png"/>

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/parameters).
