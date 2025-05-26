# ⚙️ Fase 3.2 – Introducción de SignalStore para dominios locales

### 🎯 Objetivo

Aplicar `@ngrx/signals` y `signal-store` para modelar slices de estado altamente reactivos y encapsulados dentro de componentes específicos. Se busca combinar `Store` global para datos compartidos y `SignalStore` para vistas locales como filtros o selección.

---

### 🗂️ Scaffolding

```
src/app/stores/
├── plant-filter.store.ts         ← SignalStore para el filtro local

src/app/pages/plants/
├── plant-filter.component.ts     ← usa store reactiva
```

---

### 🪜 Pasos guiados

1. Instala señal-store si no está:

```bash
npm install @ngrx/signals
```

2. Crea `PlantFilterStore`:

```ts
@Injectable()
export class PlantFilterStore extends signalStore({
  state: withState(() => ({
    search: '',
    onlyActive: false
  }))
}) {}
```

3. Crear componente `plant-filter.component.ts`:

```ts
@Component({
  selector: 'app-plant-filter',
  standalone: true,
  template: `
    <input type="text" [value]="store.search()" (input)="store.setSearch($event.target.value)" />
    <label><input type="checkbox" [checked]="store.onlyActive()" (change)="store.toggleOnlyActive()" /> Solo activas</label>
  `,
  providers: [provideStore(PlantFilterStore)]
})
export class PlantFilterComponent {
  constructor(public store: PlantFilterStore) {}
}
```

4. Usar señales derivadas en el padre:

```ts
@Component({ ... })
export class PlantListPage {
  allPlants = injectStoreSelector(selectAllPlants);
  filter = inject(PlantFilterStore);

  filtered = computed(() => this.allPlants().filter(p =>
    p.name.toLowerCase().includes(this.filter.search().toLowerCase()) &&
    (!this.filter.onlyActive() || p.status === 'active')
  ));
}
```

---

### 🎯 Retos


* **Crear un store de alertas locales para mostrar solo alertas recientes o críticas**

  > 💡 *Tip:* Define un nuevo `AlertViewStore` con estado `{ onlyCritical: boolean; recentMinutes: number }` y usa `computed()` sobre `selectAllAlerts` para filtrar localmente.

* **Sincronizar una señal local (`selectedPlant`) con un `dispatch` global**

  > 💡 *Tip:* Usa un efecto dentro del `SignalStore` que observe `selectedPlant()` y llame a `store.dispatch(selectPlant({ id }))` cuando cambie.

* **Derivar un stream que calcule el número de resultados filtrados**

  > 💡 *Tip:* Usa `computed(() => filtered().length)` si `filtered()` ya representa el resultado. Puedes usar esto para mostrar “N resultados encontrados”.

---

### ✅ Validaciones

* `SignalStore` encapsula toda la lógica de interacción local
* Las vistas usan `computed()` en lugar de lógica imperativa
* Se mantiene compatibilidad con el `Store` global

---

### 💬 Reflexión

¿Cuándo usar SignalStore y cuándo Store global? ¿Qué ventajas aporta tener slices altamente encapsulados por vista?
