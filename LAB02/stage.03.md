# ⚙️ Fase 2.3 – Refactor a Component Store de NGRX

### 🎯 Objetivo

Reemplazar la implementación manual de Redux por `@ngrx/component-store`, conservando el enfoque unidireccional pero ganando simplicidad, encapsulación y soporte oficial. Se reorganiza el estado y los efectos en un patrón más mantenible.

---

### 🗂️ Scaffolding

```
src/app/state/
├── plant.store.ts            ← nueva implementación con ComponentStore

src/app/pages/plants/
├── plants.page.ts            ← ahora usa selectores de la store
├── plant-list.component.ts   ← presentacional sin lógica
```

---

### 🪜 Pasos guiados

1. Instala la dependencia si no está:

```bash
npm install @ngrx/component-store
```

2. Refactoriza `PlantStore`:

```ts
interface PlantState {
  plants: Plant[];
  selectedId: string | null;
  filter: string;
}

@Injectable()
export class PlantStore extends ComponentStore<PlantState> {
  constructor() {
    super({ plants: [], selectedId: null, filter: '' });
  }

  readonly plants$ = this.select(state => state.plants);
  readonly selectedId$ = this.select(state => state.selectedId);
  readonly filter$ = this.select(state => state.filter);

  readonly filteredPlants$ = this.select(
    this.plants$,
    this.filter$,
    (plants, filter) => plants.filter(p => p.name.toLowerCase().includes(filter.toLowerCase()))
  );

  readonly setPlants = this.updater((state, plants: Plant[]) => ({ ...state, plants }));
  readonly setFilter = this.updater((state, filter: string) => ({ ...state, filter }));
  readonly selectPlant = this.updater((state, id: string) => ({ ...state, selectedId: id }));
  readonly clearSelection = this.updater(state => ({ ...state, selectedId: null }));
}
```

3. Inyecta la store en el componente contenedor:

```ts
@Component({...})
export class PlantsPage implements OnInit {
  readonly filteredPlants$ = this.store.filteredPlants$;
  constructor(public store: PlantStore) {}

  ngOnInit() {
    this.store.setPlants([...]); // simula carga inicial
  }
}
```

4. Enlaza `plant-list.component.ts` con `@Input()` y `@Output()`

```ts
@Component({...})
export class PlantListComponent {
  @Input() plants: Plant[] = [];
  @Output() select = new EventEmitter<string>();
}
```

---

### 🎯 Retos

* Convertir `selectedId$` en un selector memoizado

  > 💡 *Tip:* Usa `this.select(...)` con múltiples inputs y lógica condicional

* Extraer la lógica de carga desde un efecto que consuma API real

  > 💡 *Tip:* Usa `this.effect(trigger$ => trigger$.pipe(...))`

* Reutilizar los `updater()` en combinación con inputs del usuario (filtros, búsqueda)

---

### ✅ Validaciones

* Todo el estado y lógica de mutación está dentro de `PlantStore`
* Los componentes solo renderizan datos o emiten eventos
* No hay `BehaviorSubject` ni lógica de suscripción manual en ningún componente

---

### 💬 Reflexión

¿ComponentStore reduce el boilerplate frente a Redux clásico? ¿Hasta qué punto puede escalar antes de necesitar `@ngrx/store` global?
