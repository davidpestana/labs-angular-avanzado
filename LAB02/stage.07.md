# ⚙️ Fase 2.7 – Gestión avanzada: filtros, estados derivados y validación

### 🎯 Objetivo

Aplicar selectores y lógica avanzada dentro de la store para manejar filtros dinámicos, validaciones de estado, y propiedades derivadas. Se busca fortalecer la separación entre el estado fuente y las representaciones útiles para la vista.

---

### 🗂️ Scaffolding

```
src/app/state/
├── plant.store.ts            ← añade selectores y lógica derivada

src/app/pages/plants/
├── plants.page.ts            ← consume streams procesados
```

---

### 🪜 Pasos guiados

1. Añadir al estado campos derivados:

```ts
interface PlantState {
  plants: Plant[];
  selectedId: string | null;
  filter: string;
  debug: boolean;
}
```

2. Crear un selector de validación:

```ts
readonly isSelectionValid$ = this.select(
  this.plants$,
  this.selectedId$,
  (plants, id) => id ? plants.some(p => p.id === id) : false
);
```

3. Crear un stream de resumen de estado:

```ts
readonly summary$ = this.select(
  this.filteredPlants$,
  plants => ({
    total: plants.length,
    active: plants.filter(p => p.status === 'active').length
  })
);
```

4. Implementar selector combinado para la vista:

```ts
readonly vm$ = this.select({
  plants: this.filteredPlants$,
  selected: this.selectedPlant$,
  valid: this.isSelectionValid$,
  summary: this.summary$
});
```

5. Mostrar en plantilla:

```html
<p *ngIf="(vm$ | async)?.valid === false">Selección inválida</p>
<p>Total: {{ (vm$ | async)?.summary.total }}</p>
<p>Activas: {{ (vm$ | async)?.summary.active }}</p>
```

---

### 🎯 Retos

* Añadir una validación que emita un error si se intenta seleccionar una planta inexistente

  > 💡 *Tip:* Usa un efecto que revise si `selectedId` existe en `plants[]` antes de continuar

* Mostrar un banner si no hay resultados tras filtrar

* Emitir un `warning$` si hay más del 30% de plantas en estado `offline`

  > 💡 *Tip:* Usa `.filter()` y `map()` sobre `plants$` o `summary$`

---

### ✅ Validaciones

* Se generan streams derivados útiles para la presentación
* La lógica está encapsulada en la store, no en la vista
* Hay separación entre estado bruto y derivados

---

### 💬 Reflexión

¿Tiene sentido derivar todo desde el estado o conviene preprocesar desde la API? ¿Cómo evitar selectores complejos y poco testables?
