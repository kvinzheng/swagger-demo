# Swagger Intro #

#### 💥Sections ####
A typical swagger file is composed of three general sections:
- API General Info - Configuration
- Paths
- Definitions

#### 💥API General Info - configuration ####

This section provides general information about your API and
how it is configured within the HTTP protocol. Version number, license, and overall descriptions of the API is housed within this section.

These are the required fields for Swagger to work:

```
swagger: '2.0'
info:
  title:
  description:
host:
basePath:
schemes:
  - http
  - https
consumes:
  - application/json
produces:
  - application/json
  ```
- info ==> we can add arbitrary keys/values
- host ==> host where API lives
- basepath ==> the path where Swagger will hook into
- schemes, consumes, produces ==> HTTP configurations

##### 💥Definitions #####
In this section of our YAML file, we can specify schema definitions
so that we do not clutter the paths section with this information; we can just reference them.

We reference as schema definition using the `$ref` property.
We prefix the name of our definition with this text: `'#/definitions/'`
Below the Path section, we open up a definition section and actually
provide schema information.

>DEFINITION EXAMPLE
```
schema:
  $ref: '#/definitions/errorModel'
...
...
definitions:
  errorModel:
    type: object
    required:
      - code
      - message
    properties:
      code:
        type: integer
        format: int32
      message:
        type: string
```

#### 💥Schema Types ####

👉 Strings
```
schema:
  type: string
```

👉 Numbers
```
schema:
  type: integer
  format: int64
```

👉 Arrays
```
schema:
  type: array
  items:
    $ref: '#/definitions/some-object'
```

👉 Objects
```
schema:
  type: object
  required:
    - id
  properties:
    id:
      type: integer
      format: int64
    name:
      type: string
  ```


#### 💥Paths ####
In this section, we specify the routes our API accepts, which methods it supports. We must provide schema definitions for the API data.

>GET REQUEST EXAMPLE
```
/routefoo:
  get:
    description:
    produces:
    responses:
      '200':
        description:
        schema:
          type: array
          items:
            $ref: '#/definitions/some-object'
      default:
        description: unexpected error
        schema:
          $ref: '#/definitions/errorModel'

/routefoo/{id}:
    get:
      description:
      produces:
      parameters:
        - name:
          in:
          description:
          required: true
          type: integer
          format: int64
      responses:
        '200':
          description:
          schema:
            $ref: '#/definitions/some-object'
        default:
          description: unexpected error
          schema:
            $ref: '#/definitions/errorModel'
```


> POST EXAMPLE
```
/routesfoo:
    post:
        description:
        produces:
        parameters:
          - name:
            in: body
            description:
            required: true
            schema:
              $ref: '#/definitions/newObj'
        responses:
          '200':
            description:
            schema:
              $ref: '#/definitions/some-object'
          default:
            description: unexpected error
            schema:
              $ref: '#/definitions/errorModel'
```
