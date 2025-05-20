# ⚙️ Fase 2.4 – Comunicación entre contenedores desacoplados

### 🎯 Objetivo

Crear varios contenedores que interactúan con una store compartida, usando flujos unidireccionales para comunicar cambios. Refuerza la separación de responsabilidades y muestra cómo una store puede servir a múltiples vistas coordinadas.

---

### 🗂️ Scaffolding

```
src/app/state/
├── plant.store.ts

src/app/pages/
├── plants/
│   ├── plants.page.ts          ← contenedor principal
│   ├── plant-list.component.ts ← lista presentacional
│   ├── plant-detail.component.ts ← detalle reactivo
```

---

### 🪜 Pasos guiados

1. Crear `PlantDetailComponent` standalone:

```ts
@Component({...})
export class PlantDetailComponent {
  @Input() plant: Plant | null = null;
}
```

2. Exponer un selector para la planta activa:

```ts
readonly selectedPlant$ = this.select(
  this.plants$,
  this.selectedId$,
  (plants, id) => plants.find(p => p.id === id) || null
);
```

3. Usar ambos componentes en `PlantsPage`:

```ts
@Component({...})
export class PlantsPage {
  readonly plants$ = this.store.filteredPlants$;
  readonly selectedPlant$ = this.store.selectedPlant$;

  constructor(public store: PlantStore) {}

  select(id: string) {
    this.store.selectPlant(id);
  }

  clear() {
    this.store.clearSelection();
  }
}
```

4. En plantilla:

```html
<plant-list [plants]="plants$ | async" (select)="select($event)"></plant-list>
<plant-detail [plant]="selectedPlant$ | async"></plant-detail>
<button (click)="clear()">Limpiar selección</button>
```

---

### 🎯 Retos

* Permitir que un componente externo también modifique el filtro

  > 💡 *Tip:* Usa un `FilterPanelComponent` con un `@Output()` de cambio que invoque `setFilter()`

* Reutilizar `PlantListComponent` con entradas diferentes (por planta activa, por tipo, etc.)

* Sincronizar estados entre dos stores separadas usando eventos o `@Input()`/`@Output()`

---

### ✅ Validaciones

* Ambos contenedores leen y actualizan estado desde la misma store
* No hay duplicidad de lógica ni acoplamientos directos entre componentes
* La aplicación reacciona de forma coherente a las acciones del usuario desde múltiples puntos

---

### 💬 Reflexión

¿Este patrón escala a múltiples dominios? ¿En qué momento es mejor aislar stores por contexto frente a mantener una única store compartida?
