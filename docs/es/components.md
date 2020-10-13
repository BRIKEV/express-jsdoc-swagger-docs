# Components
To define `components` [schema](https://swagger.io/docs/specification/components/):

```javascript
/**
 * A song
 * @typedef {object} Song
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {number} year - The year - double
 */
```
Where:
- **typedef** is the name of the scheme and is **required**.
- The keyword `@property` is used to define the properties.
- [Type](https://swagger.io/specification/#data-types) is defined between `{}` followed by the name of the property.
- You can specify if the name is required with `.required`.
- The following options, separated between ` - `, are the description and format (optional).


```javascript
/**
 * Author model
 * @typedef {object} Author
 * @property {string} name.required - Author name
 * @property {integer} age - Author age - int64
 */
```

The schemas previously defined (*Song and Author*) can also be used as properties of another scheme:
```javascript
/**
 * Album
 * @typedef {object} Album
 * @property {Song} firstSong
 * @property {Author} author
 */
```

#### The result in UI swagger will be as follow:
<img src="./assets/components.png"/>

> You can check out more examples [here](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/components).
