# Componentes
Para definir los `componentes` [schema](https://swagger.io/docs/specification/components/):

```javascript
/**
 * A song
 * @typedef {object} Song
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {number} year - The year - double
 */
```
Donde:
- **typedef** es el nombre del esquema y es **requerido**.
- La key `property` se utiliza para definir las propiedades.
- El [tipo](https://swagger.io/specification/#data-types) se define entre `{}` seguido del nombre de la propiedad.
- Se puede especificar si el nombre es requerido con `.required`.
- Las siguientes opciones, separadas entre ` - `, son la descripción y el formato (opcional).


```javascript
/**
 * Author model
 * @typedef {object} Author
 * @property {string} name.required - Author name
 * @property {integer} age - Author age - int64
 */
```

Los esquemas anteriores (*Song y Author*) también pueden ser utilizados como propiedades de otro esquema:
```javascript
/**
 * Album
 * @typedef {object} Album
 * @property {Song} firstSong
 * @property {Author} author
 */
```

#### El resultado en la UI de swagger será:
<img src="./assets/components.png"/>

> Puedes ver más ejemplos [aquí](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/components).
