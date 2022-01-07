# Combine schemas
To combine schemas as you could do in [Swagger](https://swagger.io/docs/specification/data-models/oneof-anyof-allof-not/) you could add these comments:

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
In the example above, you can see that we are using `oneOf` and two schemas that we want to use as a response to our endpoint.

It's **important** that the criteria (`oneOf`, `anyOf` and `allOf`) must be the first value and be followed by `|` and each schema, like this:
```
{<criteria>|<schema>|...|<schemaN>}

{oneOf|IntrumentalSong|PopSong}

// Or
{IntrumentalSong|PopSong}
// one of is the default value
```

We currently support, `oneOf`, `anyOf` and `allOf`. Default value `oneOf`.

> You can extend our support to Open API full support by adding extending your own swagger.json, please visit [Extend your swagger file section](merge.md) to more info.

> To learn how define components schemas, please visit [components / schemas](components.md) section.

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/blob/master/examples/combineSchemas/index.js).
