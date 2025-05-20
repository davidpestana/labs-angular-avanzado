# ⚙️ Fase 3.3 – Modularización por features y lazy loading

### 🎯 Objetivo

Dividir la aplicación en módulos funcionales independientes y configurarlos con `StoreModule.forFeature()` y `EffectsModule.forFeature()` para aprovechar el lazy loading. Esta práctica mejora la escalabilidad, el tiempo de carga inicial y la organización por dominios.

---

### 🗂️ Scaffolding

```
src/app/features/
├── plant/
│   ├── plant.module.ts             ← módulo con Store y Effects
│   ├── state/
│   │   ├── plant.reducer.ts
│   │   ├── plant.effects.ts
│   │   ├── plant.actions.ts
│   │   ├── plant.selectors.ts
│   │   └── plant.state.ts
│   ├── pages/
│   │   └── plant-list.page.ts

src/app/app.routes.ts              ← rutas con lazy loading
```

---

### 🪜 Pasos guiados

1. Crea el módulo `PlantModule`:

```ts
@NgModule({
  imports: [
    CommonModule,
    StoreModule.forFeature('plants', reducer),
    EffectsModule.forFeature([PlantEffects]),
    RouterModule.forChild([...])
  ],
  declarations: [PlantListPage]
})
export class PlantModule {}
```

2. Define lazy loading en las rutas:

```ts
export const routes: Routes = [
  {
    path: 'plants',
    loadChildren: () => import('./features/plant/plant.module').then(m => m.PlantModule)
  }
];
```

3. Reorganiza los slices de estado:

* `plant.state.ts`: interfaz y estado inicial
* `plant.reducer.ts`: pure reducer function
* `plant.actions.ts`: acciones tipadas
* `plant.effects.ts`: lógica de carga desde API
* `plant.selectors.ts`: selectores derivados

4. Usa `select()` en la página lazy-loaded:

```ts
@Component({ ... })
export class PlantListPage {
  plants$ = this.store.select(selectFilteredPlants);
  constructor(private store: Store) {}
}
```

---

### 🎯 Retos

* **Crear un módulo `AlertModule` cargado de forma perezosa con su propio slice**

  > 💡 *Tip:* Usa `StoreModule.forFeature('alerts', alertReducer)` y `EffectsModule.forFeature([AlertEffects])`. Configura la ruta en `app.routes.ts` con `loadChildren`, por ejemplo:

  ```ts
  {
    path: 'alerts',
    loadChildren: () => import('./features/alert/alert.module').then(m => m.AlertModule)
  }
  ```

* **Mover la lógica de filtrado fuera del componente a un selector parametrizado**

  > 💡 *Tip:* Define el selector usando `createSelectorFactory().withProps()` si usas props, o crea una función `selectFilteredPlants(filter: string)` que devuelva un selector parametrizable. También puedes encapsular el filtro en un `SignalStore` si es local.

* **Aplicar `provideEffects()` y `provideStore()` si estás en standalone API**

  > 💡 *Tip:* En componentes standalone o `bootstrapApplication`, usa:

  ```ts
  import { provideState, provideEffects } from '@ngrx/store';
  providers: [
    provideState('plants', reducer),
    provideEffects(PlantEffects)
  ]
  ```


---

### ✅ Validaciones

* La carga de cada módulo se hace bajo demanda
* Cada dominio tiene su reducer, efectos y acciones aisladas
* La arquitectura se vuelve escalable y mantenible a gran escala

---

### 💬 Reflexión

¿Lazy loading mejora la organización o la complejidad? ¿Cuándo conviene agrupar múltiples slices en un feature y cuándo dividirlos?
