# ⚙️ Fase 1.7 – Generación automática de tipos desde OpenAPI

### 🎯 Objetivo

Automatizar la generación de tipos y clientes de API REST en Angular a partir del contrato OpenAPI del backend. Reforzar el tipado fuerte en servicios, modelos y peticiones sin necesidad de escribir manualmente interfaces duplicadas.

---

### 🗂️ Scaffolding

```
openapi-client/
├── api.ts
├── models/
│   ├── Alert.ts
│   ├── Plant.ts
├── apis/
│   ├── AlertApi.ts
│   ├── PlantApi.ts

src/app/services/
├── plant-data.service.ts      ← ahora consume cliente generado
```

---

### 🪜 Pasos guiados

1. Instala OpenAPI Generator CLI:

```bash
npm install @openapitools/openapi-generator-cli -D
```

2. Genera el cliente desde el archivo YAML:

```bash
npx openapi-generator-cli generate \
  -i http://localhost:3000/openapi/openapi.yaml \
  -g typescript-fetch \
  -o openapi-client
```

3. Importa el cliente generado en el servicio:

```ts
import { PlantApi, Configuration } from '../../../openapi-client';

@Injectable({ providedIn: 'root' })
export class PlantDataService {
  private api = new PlantApi(new Configuration({ basePath: 'http://localhost:3000' }));

  getPlants$(): Observable<Plant[]> {
    return from(this.api.getApiPlants()).pipe(
      catchError(() => of([]))
    );
  }
}
```

4. Refactoriza el uso de `Plant` y `Alert` con los tipos generados:

```ts
import { Plant } from '../../../openapi-client/models/Plant';
```

5. Ajusta el tipado de `plants$`, `vm$`, y cualquier otro uso a los nuevos modelos fuertes.

---

### 🎯 Retos

* Validar que el contrato OpenAPI cubre todos los campos usados en Angular

  > 💡 *Tip:* Si falta alguno, edita `openapi.yaml` y vuelve a generar el cliente

* Usar `AlertApi` para crear nuevas alertas desde Angular

  > 💡 *Tip:* Prueba a enviar un `POST` desde un botón o formulario

* Automatizar la generación con un script `generate-openapi-client.sh`

  > 💡 *Tip:* Incluye esto en tu `package.json` como script npm

---

### ✅ Validaciones

* Los tipos `Plant` y `Alert` ya no están definidos a mano
* El servicio `PlantDataService` consume `PlantApi` generado
* El código cliente se actualiza automáticamente si cambia el contrato

---

### 💬 Reflexión

¿Qué beneficios aporta tener un contrato OpenAPI real? ¿Cómo mejora el mantenimiento y la colaboración entre equipos?
