# Estenda seu arquivo swagger

Esta opção é útil se você tem um projeto antigo/legado, e você só quer adicionar comentários nos novos endpoints, ou quer adicionar novas funcionalidades que nós não suportamos.

## Exemplo
Você pode ter um arquivo `swagger.json` já definido como este:
<details><summary>Clique para expandir</summary>

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

Que gera o SwaggerUI a seguir:

<img src="./assets/merge.png"/>

Se você quer *integrar* sua api escrita com comentários `jsdoc` e seu arquivo `swagger.json`, você pode verificar este exemplo:

```javascript
const express = require('express');
const oldSwagger = require('./swagger.json'); // este aquivo contém o arquivo swagger.json mencionado anteriormente
const expressJSDocSwagger = require('express-jsdoc-swagger');

const options = {
  info: { // propriedades version e title são obrigatórios
    version: '1.0.0',
    title: 'Albums store',
  },
  filesPattern: './simple.js',
  baseDir: __dirname,
};

const app = express();
const port = 3001;

// Para combinar o aquivo swagger.json que nós tínhamos com os novos, ele será passado com um segundo parâmetro:
expressJSDocSwagger(app)(options, oldSwagger);

// Exemplo de novo endpoint:
/**
 * GET /api/v1/albums
 * @summary Este é o resumo ou descrição do endpoint
 * @tags Album
 * @param {array<object>} name.query.required - descrição do parâmetro name
 * @return {array<object>} 200 - success response - application/json
 */
app.get('/api/v1/albums', (req, res) => (
  res.json([{
    title: 'album 1',
  }])
));

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`));

```

Finalmente, o resultado no SwaggerUI a seguir:

<img src="./assets/merge-result.png"/>

> Você pode acessar mais exemplos [aqui](https://github.com/BRIKEV/express-jsdoc-swagger/tree/master/examples/merge).
