# ⚙️ Fase 1.3 – Backend Node.js con JSON como base de datos

### 🎯 Objetivo

Levantar un backend simulado en Node.js que exponga datos REST desde archivos JSON para poder consumirlos desde la aplicación Angular. Incluir una documentación OpenAPI (Swagger) que permita su generación de clientes desde Angular en fases posteriores. El backend expondrá datos realistas generados con Faker y ofrecerá funcionalidad suficiente con 100 ítems por modelo.

---

### 🗂️ Scaffolding

```
solar-api/
├── db/
│   ├── plants.json
│   └── alerts.json
├── routes/
│   ├── plants.js
│   └── alerts.js
├── server.js
├── generate.js
├── package.json
├── openapi/
│   └── openapi.yaml
├── swagger/
│   └── swagger.js
```

---

### 🪜 Pasos guiados

1. Inicializa el proyecto:

```bash
mkdir solar-api && cd solar-api
npm init -y
npm install express cors faker swagger-ui-express yamljs
```

2. Genera datos simulados (100 ítems):
   Ver `generate.js` usando Faker para `plants.json` y `alerts.json`

3. Crea el servidor con Swagger incluido:

```js
const express = require('express');
const cors = require('cors');
const swaggerUi = require('swagger-ui-express');
const YAML = require('yamljs');

const app = express();
const port = 3000;
const swaggerDocument = YAML.load('./openapi/openapi.yaml');

app.use(cors());
app.use(express.json());
app.use('/api/plants', require('./routes/plants'));
app.use('/api/alerts', require('./routes/alerts'));
app.use('/api/docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));

app.listen(port, () => {
  console.log(`API running at http://localhost:${port}`);
  console.log(`Swagger docs at http://localhost:${port}/api/docs`);
});
```

4. Define el contenido de `openapi/openapi.yaml`:

```yaml
openapi: 3.0.0
info:
  title: Solar Monitor API
  version: 1.0.0
paths:
  /api/plants:
    get:
      summary: Lista de plantas solares
      responses:
        '200':
          description: Lista de plantas
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Plant'
  /api/plants/{id}:
    get:
      summary: Obtener planta por ID
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Planta encontrada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Plant'
  /api/alerts:
    get:
      summary: Lista de alertas activas
      responses:
        '200':
          description: Lista de alertas
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Alert'
    post:
      summary: Crear una alerta
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Alert'
      responses:
        '201':
          description: Alerta creada
components:
  schemas:
    Plant:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        location:
          type: string
        capacityKw:
          type: integer
        status:
          type: string
    Alert:
      type: object
      properties:
        id:
          type: string
        plantId:
          type: string
        type:
          type: string
        message:
          type: string
        timestamp:
          type: string
```

---

### 🎯 Retos

* Añadir `GET /api/plants/:id` con documentación Swagger incluida

  > 💡 *Tip:* Define el parámetro de ruta en `openapi.yaml` bajo `parameters`

* Añadir `POST /api/alerts` documentado en Swagger

  > 💡 *Tip:* Define el body con `$ref` a un esquema `Alert`

* Añadir soporte de paginación documentado (query params `page`, `limit`)

  > 💡 *Tip:* Usa parámetros de tipo `query` definidos en Swagger

---

### ✅ Validaciones

* La documentación Swagger es accesible desde `/api/docs`
* El contrato OpenAPI incluye todos los endpoints implementados
* Angular puede consultar los datos correctamente desde el backend

---

### 💬 Reflexión

¿Swagger es útil solo para documentación? ¿Cómo se relaciona con la generación de clientes y contratos entre equipos frontend/backend?
