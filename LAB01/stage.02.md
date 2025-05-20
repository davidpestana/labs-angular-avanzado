# ⚙️ Fase 1.2 – Simulación de datos y servicio reactivo

### 🎯 Objetivo

Transformar los datos simulados del servicio en flujos observables controlados. Aplicar patrones de emisión y suscripción reactivas para preparar la vista y la lógica de negocio ante datos asincrónicos y dinámicos.

---

### 🗂️ Scaffolding

```
src/app/services/
├── plant-data.service.ts      ← refactor con BehaviorSubject y delay

src/app/pages/dashboard/
├── dashboard.page.ts          ← suscripción reactiva al servicio
├── dashboard.page.html        ← uso de async pipe, render condicional
```

---

### 🪜 Pasos guiados

1. Refactoriza `PlantDataService`:

```ts
@Injectable({ providedIn: 'root' })
export class PlantDataService {
  private _plants$ = new BehaviorSubject<any[]>([]);

  get plants$(): Observable<any[]> {
    return this._plants$.asObservable();
  }

  loadPlants(): void {
    of([
      { id: 'plant-001', name: 'Planta Solar Norte', location: 'Valencia' },
      { id: 'plant-002', name: 'Planta Solar Este', location: 'Castellón' }
    ])
    .pipe(delay(500), tap(console.log))
    .subscribe(data => this._plants$.next(data));
  }
}
```

2. Modifica `DashboardPage` para consumir `plants$`:

```ts
@Component({
  selector: 'app-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './dashboard.page.html',
})
export class DashboardPage implements OnInit {
  plants$ = this.data.plants$;
  constructor(private data: PlantDataService) {}
  ngOnInit() {
    this.data.loadPlants();
  }
}
```

3. Modifica `dashboard.page.html`:

```html
<section *ngIf="plants$ | async as plants; else loading">
  <ul>
    <li *ngFor="let plant of plants">
      {{ plant.name }} – {{ plant.location }}
    </li>
  </ul>
</section>
<ng-template #loading>
  <p>Cargando datos...</p>
</ng-template>
```

---

### 🎯 Retos

* Simular recarga automática cada 10 segundos

  > 💡 *Tip:* Usa `interval(10000).pipe(switchMap(...))` para llamar a `loadPlants()` periódicamente.

* Mostrar la hora de la última actualización

  > 💡 *Tip:* Usa otro `BehaviorSubject<string>` para guardar el `timestamp` y renderízalo en la vista.

---

### ✅ Validaciones

* Datos mostrados con `async` pipe
* No se usan suscripciones manuales
* Se muestra un mensaje de carga antes de que lleguen los datos

---

### 💬 Reflexión

¿La lógica declarativa mejora la claridad del flujo? ¿Dónde sería útil compartir este observable con otros componentes?
