# ⚙️ Fase 4.2 – Protección de rutas y visibilidad condicional por permisos

### 🎯 Objetivo

Añadir seguridad básica a nivel de rutas, componentes y lógica de interfaz en función del rol o permisos del usuario. Controlar la carga condicional de recursos y el acceso a funcionalidades de forma declarativa.

---

### 🗂️ Scaffolding

```
src/app/auth/
├── auth.guard.ts              ← control de acceso por ruta
├── role.directive.ts          ← *ngIfRole personalizado
├── auth.service.ts            ← usuario simulado y sus permisos

src/app/routes/
├── app.routes.ts              ← rutas protegidas
```

---

### 🪜 Pasos guiados

1. Simula un usuario con roles en `AuthService`:

```ts
@Injectable({ providedIn: 'root' })
export class AuthService {
  user = signal({ username: 'admin', roles: ['admin', 'viewer'] });
  hasRole = (role: string) => this.user().roles.includes(role);
}
```

2. Crea un guard `AuthGuard` para proteger rutas:

```ts
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivateFn {
  constructor(private auth: AuthService) {}
  canActivate(): boolean {
    return this.auth.hasRole('admin');
  }
}
```

3. Protege rutas en `app.routes.ts`:

```ts
{
  path: 'admin',
  canActivate: [AuthGuard],
  loadChildren: () => import('../features/admin/admin.module').then(m => m.AdminModule)
}
```

4. Añade una directiva `*ngIfRole` para visibilidad condicional:

```ts
@Directive({ selector: '[ngIfRole]' })
export class RoleDirective {
  @Input() ngIfRole!: string;
  constructor(tpl: TemplateRef<any>, vc: ViewContainerRef, auth: AuthService) {
    if (auth.hasRole(this.ngIfRole)) {
      vc.createEmbeddedView(tpl);
    }
  }
}
```

5. Usar en plantilla:

```html
<button *ngIfRole="'admin'">Eliminar</button>
```

---

### 🎯 Retos y Tips

* **Añadir múltiples niveles de acceso (admin, editor, viewer)**

  > 💡 *Tip:* Cambia el array de roles del usuario y prueba distintos escenarios desde un `role switcher` visible

* **Controlar carga condicional de un módulo entero basado en rol**

  > 💡 *Tip:* Usa `canLoad` en lugar de `canActivate` para que el bundle no se cargue si no procede

* **Mostrar contenido de forma condicional en componentes reactivos**

  > 💡 *Tip:* Expón el rol desde el `AuthService` como `computed()` o `Observable` para usar `*ngIf="(isAdmin$ | async)"`

---

### ✅ Validaciones

* Las rutas se protegen con `canActivate` o `canLoad`
* El contenido visual responde al rol del usuario
* El sistema es extensible a nuevos roles sin reescribir lógica

---

### 💬 Reflexión

¿Hasta qué punto este enfoque es suficiente para una app real? ¿Qué parte debería ir acompañada por lógica en el backend o tokens JWT? ¿La visibilidad condicional es seguridad real?
