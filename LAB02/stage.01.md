# 🔁 Fase 2.1 – Bootstrap Redux desde arquitectura RxJS (Iteración sobre Lab01)

### 🎯 Objetivo

Partir de la arquitectura RxJS del laboratorio anterior e introducir progresivamente Redux, utilizando `@ngrx/component-store` como mecanismo de gestión local de estado. Se busca preservar la lógica declarativa previa pero encapsularla dentro de una store, facilitando escalabilidad y trazabilidad.

---

### 🗂️ Scaffolding

```
src/app/pages/dashboard/
├── dashboard.page.ts            ← se convierte en contenedor
├── dashboard.store.ts           ← nueva store basada en ComponentStore
├── dashboard.page.html
```

---

### 🪜 Pasos guiados

1. Instala la librería:

```bash
npm install @ngrx/component-store
```

2. Crea `DashboardStore`:

```ts
interface DashboardState {
  plants: Plant[];
  loading: boolean;
  error: string | null;
}

@Injectable()
export class DashboardStore extends ComponentStore<DashboardState> {
  constructor(private api: PlantDataService) {
    super({ plants: [], loading: false, error: null });
  }

  readonly plants$ = this.select(state => state.plants);
  readonly loading$ =
```
