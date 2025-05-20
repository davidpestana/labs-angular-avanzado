# ⚙️ Fase 4.3 – Lazy loading profundo por dominio + preload selectivo

### 🎯 Objetivo

Optimizar el tamaño del bundle inicial de la aplicación mediante la carga perezosa profunda (`lazy loading`) de módulos por dominio y configuración avanzada del sistema de preloading selectivo (`Quicklink`, `CustomPreloadingStrategy`, etc).

---

### 🗂️ Scaffolding

```
src/app/routes/
├── app.routes.ts                ← rutas con estrategia de preloading

src/app/features/
├── plant/plant.module.ts       ← módulo cargado perezosamente
├── alert/alert.module.ts       ← módulo separado, también lazy

src/app/core/
├── strategies/custom-preloader.ts  ← estrategia de precarga selectiva
```

---

### 🪜 Pasos guiados

1. Configura los módulos por dominio con `RouterModule.forChild()` y `loadChildren()` en `app.routes.ts`:

```ts
{
  path: 'plants',
  loadChildren: () => import('../features/plant/plant.module').then(m => m.PlantModule),
  data: { preload: true }
},
{
  path: 'alerts',
  loadChildren: () => import('../features/alert/alert.module').then(m => m.AlertModule),
  data: { preload: false }
}
```

2. Implementa `CustomPreloadingStrategy`:

```ts
@Injectable({ providedIn: 'root' })
export class CustomPreloader implements PreloadingStrategy {
  preload(route: Route, load: () => Observable<any>): Observable<any> {
    return route.data?.['preload'] ? load() : of(null);
  }
}
```

3. Registra la estrategia de precarga en el `RouterModule.forRoot(...)`:

```ts
RouterModule.forRoot(routes, {
  preloadingStrategy: CustomPreloader
})
```

4. Opcional: integrar `ngx-quicklink` para precarga basada en visibilidad de enlaces:

```bash
npm install @ngx-quicklink/core
```

```ts
import { QuicklinkModule, QuicklinkStrategy } from '@ngx-quicklink/core';
...
RouterModule.forRoot(routes, { preloadingStrategy: QuicklinkStrategy })
```

---

### 🎯 Retos y Tips

* **Marcar sólo los módulos necesarios para preload**

  > 💡 *Tip:* Usa `data: { preload: true }` solo para lo más crítico (p.ej. dashboard)

* **Verificar que los bundles no se cargan hasta que se navega**

  > 💡 *Tip:* Usa pestaña Network en DevTools → filtro por `.js` → compara tráfico antes y después de aplicar lazy loading

* **Combinar `canLoad` con lazy loading para mayor seguridad**

  > 💡 *Tip:* Impide que incluso se descargue el módulo si el rol del usuario no lo permite

---

### ✅ Validaciones

* Los módulos de dominio se cargan perezosamente
* Solo los marcados con preload se descargan de antemano
* Se verifica menor carga inicial y separación lógica por contexto

---

### 💬 Reflexión

¿Qué diferencia real produce el preload selectivo en UX? ¿Cuándo priorizar rendimiento inicial vs tiempo de navegación? ¿Qué problemas aparecen si abusas del lazy loading?
