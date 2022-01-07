# Componentes

Para definir [esquema](https://swagger.io/docs/specification/components/) de `componentes`:

```javascript
/**
 * A song
 * @typedef {object} Song
 * @property {string} title.required - The title
 * @property {string} artist - The artist
 * @property {number} year - The year - double
 */
```

Onde:

- **typedef** é o nome do esquema e é **obrigatório**.
- A palavra chave `@property` é usada para definir as propriedades.
- [Tipo](https://swagger.io/specification/#data-types) é definido entre `{}` seguido pelo nome da propriedade.
- Você pode especificar se a propriedade é obrigatória com `.required`.
- A opções seguintes, separadas por `-`, são a descrição e formato (opcional).

```javascript
/**
 * Author model
 * @typedef {object} Author
 * @property {string} name.required - Author name
 * @property {integer} age - Author age - int64
 */
```

Os esquemas definidos previamente (_Musica and Autor_) podem ser usados como propriedades de outro esquema:

```javascript
/**
 * Album
 * @typedef {object} Album
 * @property {Song} firstSong
 * @property {Author} author
 */
```

#### O resultado no swagger será como o seguinte:

<img src="./assets/components.png"/>

> Você pode encontrar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/components).
