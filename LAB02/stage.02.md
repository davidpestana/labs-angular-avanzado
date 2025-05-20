# ⚙️ Fase 2.2 – Introducción del patrón Redux manual

### 🎯 Objetivo

Modelar manualmente el patrón Redux con `BehaviorSubject`, separando acciones, estado y reducers sin librerías externas. Entender cómo se estructura un flujo unidireccional desde cero.

---

### 🗂️ Scaffolding

```
src/app/state/
├── plant.actions.ts        ← definición de acciones
├── plant.reducer.ts        ← lógica pura de transición de estado
├── plant.state.ts          ← definición de interfaz de estado
├── plant.store.ts          ← Subject + Dispatcher

src/app/pages/plants/
├── plants.page.ts          ← contenedor smart
├── plant-list.component.ts ← presentacional
```

---

### 🪜 Pasos guiados

1. Define el estado:

```ts
export interface PlantState {
  plants: Plant[];
  selectedId: string | null;
  filter: string;
}
```

2. Define las acciones como tipos discriminados:

```ts
export type PlantActions =
  | { type: 'LOAD_PLANTS'; payload: Plant[] }
  | { type: 'SET_FILTER'; payload: string }
  | { type: 'SELECT_PLANT'; payload: string };
```

3. Crea el reducer:

```ts
export function plantReducer(state: PlantState, action: PlantActions): PlantState {
  switch (action.type) {
    case 'LOAD_PLANTS': return { ...state, plants: action.payload };
    case 'SET_FILTER': return { ...state, filter: action.payload };
    case 'SELECT_PLANT': return { ...state, selectedId: action.payload };
    default: return state;
  }
}
```

4. Implementa el store:

```ts
@Injectable({ providedIn: 'root' })
export class PlantStore {
  private initialState: PlantState = { plants: [], selectedId: null, filter: '' };
  private state$ = new BehaviorSubject<PlantState>(this.initialState);

  readonly state = this.state$.asObservable();
  readonly plants$ = this.state.pipe(map(s => s.plants));
  readonly selected$ = this.state.pipe(map(s => s.selectedId));

  dispatch(action: PlantActions) {
    const current = this.state$.value;
    this.state$.next(plantReducer(current, action));
  }
}
```

5. Usa el store en `plants.page.ts`:

```ts
export class PlantsPage implements OnInit {
  readonly plants$ = this.store.plants$;
  constructor(private store: PlantStore) {}

  ngOnInit() {
    this.store.dispatch({ type: 'LOAD_PLANTS', payload: [...] });
  }
}
```

---

### 🎯 Retos

* Añadir acción `CLEAR_SELECTION`

  > 💡 *Tip:* Devuelve `selectedId: null` desde el reducer

* Añadir un selector `filteredPlants$` que combine `plants` y `filter`

  > 💡 *Tip:* Usa `combineLatest` y `filter.includes()`

* Mostrar planta seleccionada en un componente hijo `PlantDetailComponent`

---

### ✅ Validaciones

* No hay lógica de estado en el componente
* El estado se actualiza solo a través del reducer
* Los observables de estado se derivan correctamente del stream principal

---

### 💬 Reflexión

¿Qué ventajas y limitaciones tiene implementar Redux a mano? ¿Qué cambiaría al migrar a una librería como NGRX?
