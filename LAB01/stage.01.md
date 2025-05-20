# ⚙️ Fase 1.1 – Inicialización del proyecto y layout base

### 🎯 Objetivo

Crear la base del proyecto Angular standalone con un layout visual claro, enrutamiento inicial, páginas principales y un servicio de datos simulado.

---

### 🗂️ Scaffolding

```
solar-monitor/
├── src/
│   ├── app/
│   │   ├── app.config.ts
│   │   ├── app.routes.ts
│   │   ├── layout/
│   │   │   ├── layout.component.ts
│   │   │   ├── layout.component.html
│   │   │   ├── layout.component.css
│   │   ├── pages/
│   │   │   ├── dashboard/
│   │   │   │   ├── dashboard.page.ts
│   │   │   │   ├── dashboard.page.html
│   │   │   │   ├── dashboard.page.css
│   │   │   ├── plants/
│   │   │   │   ├── plants.page.ts
│   │   │   │   ├── plants.page.html
│   │   │   │   ├── plants.page.css
│   │   ├── services/
│   │   │   ├── plant-data.service.ts
│   ├── main.ts
```

---

### 🪜 Pasos guiados

1. Crear el proyecto:

   ```bash
   ng new solar-monitor --standalone --routing --style=css
   ```

2. Crear `LayoutComponent` standalone:

```ts
@Component({
  selector: 'app-layout',
  standalone: true,
  imports: [CommonModule, RouterModule],
  templateUrl: './layout.component.html',
  styleUrls: ['./layout.component.css']
})
export class LayoutComponent {}
```

3. `layout.component.html`:

```html
<header>
  <h1>Solar Monitor</h1>
  <nav>
    <a routerLink="/" routerLinkActive="active">Dashboard</a>
    <a routerLink="/plants" routerLinkActive="active">Plantas</a>
  </nav>
</header>
<main>
  <router-outlet></router-outlet>
</main>
```

4. Crear `DashboardPage` y `PlantsPage` como standalone con texto estático.

5. Crear `app.routes.ts`:

```ts
export const routes: Routes = [
  { path: '', component: DashboardPage },
  { path: 'plants', component: PlantsPage },
  { path: '**', redirectTo: '' }
];
```

6. Crear `PlantDataService` con método simulado:

```ts
@Injectable({ providedIn: 'root' })
export class PlantDataService {
  getPlants(): Observable<any[]> {
    return of([
      { id: 'plant-001', name: 'Planta Solar Norte', location: 'Valencia' },
      { id: 'plant-002', name: 'Planta Solar Este', location: 'Castellón' }
    ]);
  }
}
```

---

### 🎯 Retos

* Añadir `AlertsPage` con ruta `/alerts`

  > 💡 *Tip:* Reutiliza la estructura de `DashboardPage`. Puedes incluir un mensaje de alerta estático y dejarlo preparado para consumir datos más adelante.

* Estilizar la cabecera y navegación

  > 💡 *Tip:* Usa Flexbox para alinear el contenido del `header`. Prueba a usar `gap`, `justify-content: space-between` y un color de fondo suave.

* Implementar menú responsive colapsable con CSS puro

  > 💡 *Tip:* Usa una `media query` y una clase `hidden` que se active con `@media (max-width: 768px)`. Puedes utilizar un `<button>` para mostrar/ocultar el menú con `ngClass` si lo necesitas.

---

### ✅ Validaciones

* Navegación entre páginas operativa
* Layout fijo y limpio
* Datos simulados accesibles desde el servicio

---

### 💬 Reflexión

¿Se ha separado correctamente la estructura lógica y visual? ¿Dónde conviene modularizar desde el inicio?
