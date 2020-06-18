# Merge

This option is useful if you have an old project and only want to comment on new endpoints being developed.
## collapsible markdown?

### Example
You may have previously defined the following:
<details><summary>Click to expand</summary>

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

### Which gives us the next output:

<img src="./assets/merge.png"/>

If you don't want to lose/migrate that json, you have the next option:

> If you want to know more about *express-jsdoc-swagger* configuration, please visit [configuration](configuration.md) section.

Now, the above json can be included in a file called swagger.js, to be imported later.

## Full integration example
```javascript
const express = require('express');
const oldSwagger = require('./swagger');
const expressJSDocSwagger = require('express-jsdoc-swagger');

const options = {
  info: { // Where version and title are required
    version: '1.0.0',
    title: 'Albums store',
  },
  filesPattern: './simple.js',
  baseDir: __dirname,
};

const app = express();
const port = 3001;

// To merge the json we had with the new endpoints, It will be passed as a second parameter:
expressJSDocSwagger(app)(options, oldSwagger);

// Example of new endpoint:
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

app.listen(port, () => logger.info(`Example app listening at http://localhost:${port}`));

```

Finally, the outcome is:

<img src="./assets/merge-result.png"/>
