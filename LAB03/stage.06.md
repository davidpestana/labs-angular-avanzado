# ⚙️ Fase 3.6 – Devtools, trazabilidad y testing de reducers/effects

### 🎯 Objetivo

Activar herramientas de desarrollo (`@ngrx/store-devtools`) para trazar el flujo de acciones y validar el estado. Implementar pruebas unitarias de reducers y efectos usando `provideMockStore` y `provideMockActions`.

---

### 🗂️ Scaffolding

```
src/app/core/state/
├── app.config.ts                 ← añade StoreDevtools

src/app/features/plant/state/
├── plant.reducer.spec.ts         ← test unitario
├── plant.effects.spec.ts         ← test de efectos
```

---

### 🪜 Pasos guiados

1. Instalar DevTools:

```bash
npm install @ngrx/store-devtools
```

2. Añadir a la configuración:

```ts
import { provideStoreDevtools } from '@ngrx/store-devtools';

bootstrapApplication(AppComponent, {
  providers: [
    provideStore(...),
    provideStoreDevtools({ maxAge: 25, logOnly: environment.production })
  ]
})
```

3. Escribir test de `plant.reducer.ts`:

```ts
describe('PlantReducer', () => {
  it('should load plants', () => {
    const state = reducer(initialState, loadPlantsSuccess({ plants: mockPlants }));
    expect(state.plants.length).toBeGreaterThan(0);
  });
});
```

4. Escribir test de `plant.effects.ts`:

```ts
provideMockStore();
provideMockActions(() => actions$);

it('should call loadPlants API and dispatch success', () => {
  actions$ = hot('-a', { a: loadPlants() });
  const expected = cold('-b', { b: loadPlantsSuccess({ plants: mockPlants }) });
  expect(effects.loadPlants$).toBeObservable(expected);
});
```

---

### 🎯 Retos

* Validar que no se disparen acciones duplicadas en efectos asincrónicos

  > 💡 *Tip:* Usa `exhaustMap` o `takeUntil(nextAction)` en lugar de `switchMap` si hay riesgo de concurrencia

* Comprobar que el selector memoiza correctamente con `resultMemoizedSelectorCreator`

  > 💡 *Tip:* Usa `selector.projector(...)` en tests unitarios para validar derivaciones

* Simular un fallo del API y validar que se despacha `loadPlantsFailure()`

  > 💡 *Tip:* Usa `throwError(() => new Error(...))` dentro de un `cold()` en el test

---

### ✅ Validaciones

* El Devtools permite inspeccionar acciones y difs del estado
* Los reducers están cubiertos por tests que verifican lógica pura
* Los efectos reaccionan correctamente al flujo de acciones simuladas

---

### 💬 Reflexión

¿Las Devtools ayudan en entornos productivos o son útiles solo en desarrollo? ¿Cuál es la diferencia entre testear selectors, reducers y efectos?
