# Request body
To add a [body payload](https://swagger.io/docs/specification/describing-request-body/) to your endpoints with express-jsdoc-swagger, you could add these comments:

```javascript
/**
 * POST /api/v1/album
 * @param {object} request.body.required - songs info - application/json
 */
app.post('/api/v1/album', (req, res) => res.send('You save a song!'));
```

Where:
- `@param` is used to define one parameter.
- [Type](https://swagger.io/specification/#data-types) is defined between `{}`.
- After the type, you have to define the key **request.body**. This key must have that value if you want this as a body payload.
- The following option, separated between ` - `, is the description.
- The last option of the keyword `@param` *(application/json)* specify the request media type. This is optional and its default value is *application/json*.

You can mix this with [components](components.md) to define a payload based on a component like in this example:

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

Or you can add form-data requests to upload files:

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

In that example, we use the last option of the keyword `@param` to specify the request media type as multipart/form-data.

The result in swagger UI will be this:

<img src="../assets/request-body.png"/>

> To learn how to add examples of your endpoint's payload, check out the [examples](examples.md) section.

> To learn how define components schemas, please visit [components](components.md) section.

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/requestBody).

### Body as form parameter

You can send some body parameters without using a component. To do that you could add these comments:

```javascript
/**
 * POST /api/v1/song
 * @param {string} id.form.required - This is the song id - application/x-www-form-urlencoded
 * @param {string} title.form.required - This is the song title - application/x-www-form-urlencoded
 * @return {object} 200 - song response
 */
app.post('/api/v1/songs', (req, res) => res.json({}));
```

Where:
- `@param` is used to define one parameter.
- [Type](https://swagger.io/specification/#data-types) is defined between `{}`.
- After the type, you have to define the key you want for the parameter followed by **form** value.
- The following option, separated between ` - `, is the description.
- The last option of the keyword `@param` *(application/json)* specify the request media type. This is optional and its default value is *application/json*.

**IMPORTANT:** To use form params it is neccesary to provide a description for each field

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/requestBody/formParameters.js).
