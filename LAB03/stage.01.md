# ⚙️ Fase 3.1 – Setup global con @ngrx/store y @ngrx/effects

### 🎯 Objetivo

Configurar el `StoreModule` y `EffectsModule` a nivel global. Crear los slices iniciales del estado global (ej. `plants`, `alerts`) con sus reducers, efectos y acciones. La aplicación Angular usará ahora un store centralizado para orquestar la carga y el consumo de datos desde la API.

---

### 🗂️ Scaffolding

```
src/app/
├── app.module.ts
├── app.state.ts              ← RootState + root reducer
├── store/
│   ├── plants/
│   │   ├── plants.actions.ts
│   │   ├── plants.reducer.ts
│   │   ├── plants.effects.ts
│   │   ├── plants.selectors.ts
│   └── alerts/
│       ├── alerts.actions.ts
│       ├── alerts.reducer.ts
│       ├── alerts.effects.ts
│       ├── alerts.selectors.ts
```

---

### 🪜 Pasos guiados

1. Instalar dependencias necesarias:

```bash
npm install @ngrx/store @ngrx/effects @ngrx/store-devtools
```

2. Definir el `AppState` y combinar los reducers:

```ts
export interface AppState {
  plants: PlantState;
  alerts: AlertState;
}

export const reducers: ActionReducerMap<AppState> = {
  plants: plantReducer,
  alerts: alertReducer,
};
```

3. Configurar el módulo raíz:

```ts
@NgModule({
  imports: [
    StoreModule.forRoot(reducers),
    EffectsModule.forRoot([PlantEffects, AlertEffects]),
    StoreDevtoolsModule.instrument({ maxAge: 25 })
  ]
})
export class AppModule {}
```

4. Crear acciones, reducers y efectos de `plants`:

```ts
// plants.actions.ts
export const loadPlants = createAction('[Plant] Load');
export const loadPlantsSuccess = createAction('[Plant] Load Success', props<{ plants: Plant[] }>());

// plants.reducer.ts
export const initialState: PlantState = { data: [], loading: false };
export const plantReducer = createReducer(
  initialState,
  on(loadPlants, state => ({ ...state, loading: true })),
  on(loadPlantsSuccess, (state, { plants }) => ({ ...state, data: plants, loading: false }))
);

// plants.effects.ts
@Injectable()
export class PlantEffects {
  load$ = createEffect(() => this.actions$.pipe(
    ofType(loadPlants),
    switchMap(() => this.api.getAll().pipe(
      map(plants => loadPlantsSuccess({ plants }))
    ))
  ));
  constructor(private actions$: Actions, private api: PlantApiService) {}
}
```

---

### 🎯 Retos

* **Crear un slice `alerts` con su propio reducer, estado inicial, acciones y efecto de carga desde API**

  > 💡 *Tip:* Reutiliza el patrón de `plants`: define `alerts.reducer.ts`, `alerts.actions.ts`, `alerts.effects.ts` y registra con `StoreModule.forFeature('alerts', reducer)` y `EffectsModule.forFeature([AlertEffects])`.

* **Crear un selector combinado que muestre número total de alertas por tipo**

  > 💡 *Tip:* Usa `createSelector(selectAllAlerts, alerts => groupBy(alerts, a => a.type))` y convierte cada grupo en un array de `{ type, count }`.

* **Refactorizar la carga inicial de `plants` para que se dispare automáticamente al arrancar la app**

  > 💡 *Tip:* Usa un efecto que escuche `init$ = createEffect(() => this.actions$.pipe(ofType(ROOT_EFFECTS_INIT), ...))` o lanza `loadPlants` desde `AppComponent` en `ngOnInit()`.


---

### ✅ Validaciones

* `StoreModule` y `EffectsModule` están configurados a nivel raíz
* Los reducers reciben y procesan las acciones esperadas
* La API es consultada mediante efectos, no desde el componente
* Los componentes obtienen datos del store mediante selectores

---

### 💬 Reflexión

¿La separación de concerns entre efecto, reducer y selector es más robusta o más compleja? ¿Qué diferencias hay con `ComponentStore`? ¿Se hace más difícil de mantener o más escalable?
