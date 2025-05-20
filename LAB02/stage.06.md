# ⚙️ Fase 2.6 – Refactor de servicios hacia acciones explícitas

### 🎯 Objetivo

Reemplazar el uso directo de servicios en componentes por una lógica basada únicamente en acciones y efectos definidos en la store. Todo el flujo de datos y efectos externos se gestiona de forma declarativa desde la `ComponentStore`.

---

### 🗂️ Scaffolding

```
src/app/state/
├── plant.store.ts            ← centraliza lógica de acciones y efectos

src/app/pages/plants/
├── plants.page.ts            ← elimina uso directo de servicios

src/app/services/
├── plant-api.service.ts      ← utilizado solo por la store
```

---

### 🪜 Pasos guiados

1. Elimina del componente toda lógica que invoque servicios directamente.

   * En su lugar, llama métodos de la store como `load()`, `select(id)` o `reset()`.

2. En la store, crea un método público por cada acción de usuario:

```ts
load(): void {
  this.loadPlants(); // llama al efecto interno
}

select(id: string): void {
  this.selectPlant(id);
  this.logSelection(id);
}

filter(term: string): void {
  this.setFilter(term);
}
```

3. Reescribe `plants.page.ts` para usar solo métodos de store:

```ts
export class PlantsPage implements OnInit {
  plants$ = this.store.filteredPlants$;
  constructor(public store: PlantStore) {}
  ngOnInit() {
    this.store.load();
  }
}
```

4. En la plantilla:

```html
<plant-list [plants]="plants$ | async" (select)="store.select($event)"></plant-list>
<input (input)="store.filter($event.target.value)" placeholder="Filtrar...">
```

---

### 🎯 Retos

* Añadir una acción pública `reset()` que limpie el estado

  > 💡 *Tip:* Usa `this.setState(defaultState)` dentro de la store

* Reubicar cualquier `console.log` en efectos o selectores trazables

* Añadir un `toggleDebug()` que active/desactive logs desde un booleano en el estado

---

### ✅ Validaciones

* Los componentes no contienen llamadas a servicios
* Toda la interacción con la lógica pasa por la store
* El comportamiento se puede testear desde la store sin acceder a la vista

---

### 💬 Reflexión

¿Esta organización mejora la mantenibilidad y testabilidad? ¿Cómo se vería afectada una app con múltiples stores interdependientes?
