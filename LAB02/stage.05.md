# ⚙️ Fase 2.5 – Gestión de side-effects y separación de comandos

### 🎯 Objetivo

Introducir efectos (`effect()`) en `ComponentStore` para gestionar operaciones con efectos secundarios como llamadas a API, logs, sincronización o navegación. Reforzar la idea de que las mutaciones del estado deben mantenerse puras y separadas de los efectos externos.

---

### 🗂️ Scaffolding

```
src/app/state/
├── plant.store.ts          ← define efectos asincrónicos y mutaciones puras

src/app/services/
├── plant-api.service.ts    ← simula servicio externo real (API REST)
```

---

### 🪜 Pasos guiados

1. Crear un servicio `PlantApiService`:

```ts
@Injectable({ providedIn: 'root' })
export class PlantApiService {
  getAll(): Observable<Plant[]> {
    return this.http.get<Plant[]>('http://localhost:3000/api/plants').pipe(delay(300));
  }

  logSelection(id: string): void {
    console.log('[log] Planta seleccionada:', id);
  }

  constructor(private http: HttpClient) {}
}
```

2. Crear efecto `loadPlants` en `PlantStore`:

```ts
readonly loadPlants = this.effect<void>(trigger$ => trigger$.pipe(
  tap(() => this.patchState({ loading: true, error: null })),
  switchMap(() => this.api.getAll().pipe(
    tapResponse(
      plants => this.patchState({ plants, loading: false }),
      error => this.patchState({ error: 'Error al cargar plantas', loading: false })
    )
  ))
));
```

3. Crear efecto `logSelection`:

```ts
readonly logSelection = this.effect<string>(id$ => id$.pipe(
  tap(id => this.api.logSelection(id))
));
```

4. Encadenar `selectPlant()` y `logSelection()`:

```ts
readonly selectPlant = this.updater((state, id: string) => ({ ...state, selectedId: id }));

select(id: string) {
  this.selectPlant(id);
  this.logSelection(id);
}
```

---

### 🎯 Retos

* Añadir efecto `loadPlantById(id)` con `switchMap()`

  > 💡 *Tip:* Útil para casos donde solo se necesita una planta individual

* Crear un `logError` que registre en consola el contenido del estado si hay error

* Añadir indicador visual reactivo de `loading$` y `error$` en la vista

---

### ✅ Validaciones

* Las llamadas externas están completamente encapsuladas como efectos
* Las mutaciones del estado son puras (sin `tap`, `subscribe` ni lógica ajena)
* El código es más legible y mantenible

---

### 💬 Reflexión

¿Los efectos mejoran la trazabilidad o añaden complejidad? ¿Hasta qué punto se pueden reutilizar efectos entre múltiples stores o dominios?
