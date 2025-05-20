# ⚙️ Fase 3.4 – Coordinación entre stores: selección cruzada y composición

### 🎯 Objetivo

Permitir que distintos slices del store colaboren de forma coordinada. Se aprenderá a componer selectores de diferentes dominios (ej. `plants` y `alerts`) y a reaccionar a cambios desde efectos que dependen de múltiples partes del estado.

---

### 🗂️ Scaffolding

```
src/app/features/
├── plant/state/
│   ├── plant.selectors.ts

├── alert/state/
│   ├── alert.selectors.ts
│   ├── alert.effects.ts

src/app/shared/
├── state/
│   ├── selectors.ts         ← selectores compuestos
```

---

### 🪜 Pasos guiados

1. Crear selector global compuesto:

```ts
export const selectPlantsWithAlerts = createSelector(
  selectAllPlants,
  selectAllAlerts,
  (plants, alerts) => plants.map(plant => ({
    ...plant,
    alerts: alerts.filter(a => a.plantId === plant.id)
  }))
);
```

2. Usarlo en la vista:

```ts
@Component({...})
export class PlantListPage {
  items$ = this.store.select(selectPlantsWithAlerts);
}
```

3. Usar `concatLatestFrom` en un efecto para reaccionar al estado combinado:

```ts
loadCriticalAlerts$ = createEffect(() => this.actions$.pipe(
  ofType(loadAlerts),
  concatLatestFrom(() => this.store.select(selectPlantFilter)),
  switchMap(([_, filter]) =>
    this.alertService.load({ criticalOnly: filter.critical }).pipe(...)
  )
));
```

---

### 🎯 Retos

* **Crear un selector que agrupe alertas por tipo a partir del slice de alertas**

  > 💡 *Tip:* Usa `createSelector(selectAllAlerts, alerts => groupBy(alerts, a => a.type))`. Puedes crear una utilidad `groupBy` genérica que devuelva `{ [type: string]: Alert[] }`.

* **Crear un efecto que escuche `selectPlant` y automáticamente cargue alertas solo para esa planta**

  > 💡 *Tip:* Usa `ofType(selectPlant)` y `switchMap(action => this.alertService.loadForPlant(action.payload))`. Asegúrate de que las alertas estén parametrizadas por `plantId`.

* **Derivar un resumen `plant.alertCount` como propiedad computada desde el selector**

  > 💡 *Tip:* Crea un selector que use `selectPlantsWithAlerts` y derive `alerts.length` por cada `plant`. Puedes extender cada planta con una propiedad `alertCount`.

---

### ✅ Validaciones

* Se usan múltiples slices del estado en selectores compuestos
* Los efectos pueden acceder al estado reactivo sin acoplar lógica
* La arquitectura mantiene cohesión entre dominios sin mezclarlos

---

### 💬 Reflexión

¿Los selectores globales pueden volverse demasiado complejos? ¿Cómo modularizar la lógica compuesta sin duplicar datos entre dominios?
