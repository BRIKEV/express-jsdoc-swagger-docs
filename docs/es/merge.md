# Extender tu archivo de swagger

Esta opción es útil si tienes un proyecto antiguo, y sólo quieres añadir comentarios sobre nuevos `endpoints`, o quieres añadir nuevas características que no soportemos.

## Ejemplo
Podrías tener un `swagger.json` ya definido como este:
<details><summary>Click para verlo completo</summary>

```js
{
  "openapi": "3.0.0",
  "info": {
    "version": "1.0.0",
    "title": "Album store",
    "license": {
      "name": "MIT"
    }
  },
  "servers": [
    {
      "url": "http://example.com"
    }
  ],
  "tags": [
    {
      "name": "Songs",
      "description": "Everything about songs"
    }
  ],
  "paths": {
    "/songs": {
      "get": {
        "operationId": "songList",
        "tags": [
          "Songs"
        ],
        "responses": {
          "200": {
            "description": "songs array",
            "headers": {
              "x-next": {
                "description": "A link to the next page of responses",
                "schema": {
                  "type": "string"
                }
              }
            },
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Songs"
                }
              }
            }
          },
          "default": {
            "description": "unexpected error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Error"
                }
              }
            }
          }
        }
      },
      "post": {
        "summary": "Create a song",
        "security": [
          {
            "BasicAuth": []
          }
        ],
        "operationId": "createSong",
        "tags": [
          "Songs"
        ],
        "responses": {
          "201": {
            "description": "Null response"
          },
          "default": {
            "description": "unexpected error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Error"
                }
              }
            }
          }
        }
      }
    },
    "/song/{songId}": {
      "get": {
        "summary": "Info for a song",
        "operationId": "showSongById",
        "tags": [
          "Songs"
        ],
        "parameters": [
          {
            "name": "songId",
            "in": "path",
            "required": true,
            "description": "The id of the song to retrieve",
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Expected response to a valid request",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Song"
                }
              }
            }
          },
          "default": {
            "description": "unexpected error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Error"
                }
              }
            }
          }
        }
      }
    }
  },
  "security": [
    {
      "BasicAuth": []
    },
    {
      "bearerAuth": []
    }
  ],
  "components": {
    "securitySchemes": {
      "BasicAuth": {
        "type": "http",
        "scheme": "basic"
      },
      "bearerAuth": {
        "type": "http",
        "scheme": "bearer",
        "bearerFormat": "JWT"
      }
    },
    "schemas": {
      "Song": {
        "type": "object",
        "required": [
          "id",
          "name"
        ],
        "properties": {
          "id": {
            "type": "integer",
            "format": "int64"
          },
          "name": {
            "type": "string"
          }
        }
      },
      "Songs": {
        "type": "object",
        "properties": {
          "song": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/Song"
            }
          }
        }
      },
      "Error": {
        "type": "object",
        "required": [
          "code",
          "message"
        ],
        "properties": {
          "code": {
            "type": "integer",
            "format": "int32"
          },
          "message": {
            "type": "string"
          }
        }
      }
    }
  }
}
```
</details>

El cual genera el siguiente swagger:

<img src="./assets/merge.png"/>

Para *integrar* tu API escrita con comentarios `jsdoc` y tu `swagger.json`, puedes ver el siguiente ejemplo:

```javascript
const express = require('express');
const oldSwagger = require('./swagger.json'); // Este archivo contiene el swagger.json que hemos mencionado antes.
const expressJSDocSwagger = require('express-jsdoc-swagger');

const options = {
  info: { // version and title are required
    version: '1.0.0',
    title: 'Albums store',
  },
  filesPattern: './simple.js',
  baseDir: __dirname,
};

const app = express();
const port = 3001;

// Para integrar el swagger.json que teníamos con los nuevos endpoints, este se pasará como segundo parámetro:
expressJSDocSwagger(app)(options, oldSwagger);

// Ejemplo de nuevos endpoints documentados con la librería:
/**
 * GET /api/v1/albums
 * @summary This is the summary or description of the endpoint
 * @tags Album
 * @param {array<object>} name.query.required - name param description
 * @return {array<object>} 200 - success response - application/json
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'abum 1',
  }])
));

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`));

```

El swagger resultante será el siguiente:

<img src="./assets/merge-result.png"/>

> Puedes ver más ejemplos [aquí](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/merge).
