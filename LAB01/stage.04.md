# ⚙️ Fase 1.4 – Integración Angular ↔ API con HttpClient y RxJS

### 🎯 Objetivo

Consumir el backend desarrollado en la fase anterior desde Angular utilizando `HttpClient`, integrándolo con flujos RxJS. Aprender a gestionar asincronía de forma reactiva, preparar el servicio para reusabilidad, y manejar errores, loaders y estados.

---

### 🗂️ Scaffolding

```
src/app/services/
├── plant-data.service.ts      ← se conecta al backend

src/app/pages/dashboard/
├── dashboard.page.ts
├── dashboard.page.html
```

---

### 🪜 Pasos guiados

1. Asegúrate de tener el backend ejecutando en `http://localhost:3000`

2. Modifica `PlantDataService` para usar `HttpClient`:

```ts
@Injectable({ providedIn: 'root' })
export class PlantDataService {
  private apiUrl = 'http://localhost:3000/api/plants';

  constructor(private http: HttpClient) {}

  getPlants$(): Observable<Plant[]> {
    return this.http.get<Plant[]>(this.apiUrl).pipe(
      catchError(err => {
        console.error('Error loading plants', err);
        return of([]);
      }),
      shareReplay(1)
    );
  }
}
```

3. Define el modelo `Plant` si aún no existe:

```ts
export interface Plant {
  id: string;
  name: string;
  location: string;
  capacityKw: number;
  status: string;
}
```

4. Modifica `DashboardPage`:

```ts
export class DashboardPage implements OnInit {
  plants$!: Observable<Plant[]>;
  constructor(private service: PlantDataService) {}
  ngOnInit() {
    this.plants$ = this.service.getPlants$();
  }
}
```

5. En la plantilla `dashboard.page.html`:

```html
<section *ngIf="plants$ | async as plants; else loading">
  <ul>
    <li *ngFor="let plant of plants">
      {{ plant.name }} ({{ plant.status }}) – {{ plant.capacityKw }} kW
    </li>
  </ul>
</section>
<ng-template #loading>
  <p>Cargando datos desde el servidor...</p>
</ng-template>
```

---

### 🎯 Retos

* Mostrar un mensaje de error si la API no responde

  > 💡 *Tip:* Añade un `Subject<string>` para emitir errores y muéstralo en plantilla con `*ngIf`

* Aplicar un `delay(500)` simulado en el observable para ver claramente el estado de carga

  > 💡 *Tip:* Añade `.pipe(delay(500))` antes de `shareReplay()`

* Mostrar cuántas plantas se están cargando

  > 💡 *Tip:* Usa `.map(plants => plants.length)` y renderízalo

---

### ✅ Validaciones

* Se consultan los datos desde la API real, no simulada
* Se gestionan correctamente los estados de carga y error
* La vista se actualiza reactivamente con los datos del backend

---

### 💬 Reflexión

¿Qué diferencia hay entre consumir datos simulados y reales? ¿Qué ventaja aporta `shareReplay()` en este contexto?
