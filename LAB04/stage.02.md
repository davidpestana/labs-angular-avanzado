### ⚙️ Fase 4.2 – Protección de rutas con guards y roles

#### 🎯 Objetivo

Aplicar protección efectiva sobre rutas Angular utilizando `canActivate`, para restringir el acceso a rutas específicas según el estado de autenticación y el rol del usuario simulado.

---

#### 🗂️ Scaffolding

```
src/app/core/auth/
├── auth.guard.ts             ← protección general por login
├── role.guard.ts             ← protección específica por rol

src/app/pages/
├── dashboard/                ← solo accesible para rol 'admin'
├── viewer-page/              ← accesible solo para 'viewer'

src/app/app.routes.ts
```

---

#### 🪜 Pasos guiados

1. **Crear `AuthGuard`** – Verifica si hay sesión activa:

```ts
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  constructor(private auth: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (!this.auth.isAuthenticated()) {
      this.router.navigate(['/']);
      return false;
    }
    return true;
  }
}
```

2. **Crear `RoleGuard`** – Verifica si el usuario tiene un rol requerido:

```ts
@Injectable({ providedIn: 'root' })
export class RoleGuard implements CanActivate {
  constructor(private auth: AuthService, private router: Router) {}

  canActivate(route: ActivatedRouteSnapshot): boolean {
    const expectedRole = route.data['role'];
    const user = this.auth.currentUser();
    if (!user || user.role !== expectedRole) {
      this.router.navigate(['/']);
      return false;
    }
    return true;
  }
}
```

3. **Proteger rutas en `app.routes.ts`**:

```ts
{
  path: 'dashboard',
  canActivate: [AuthGuard, RoleGuard],
  data: { role: 'admin' },
  loadComponent: () =>
    import('./pages/dashboard/dashboard.component').then(m => m.DashboardComponent)
}
```

---

#### 🎯 Retos y Tips

* **Crear un `canLoad` para proteger rutas lazy**
  💡 *Tip:* Extiende `AuthGuard` implementando también `CanLoad`. Angular ejecuta `canLoad` antes de cargar el módulo.

* **Permitir múltiples roles en una sola ruta**
  💡 *Tip:* Pasa un array a `data.roles` y verifica con `includes(user.role)`.

* **Evitar fallback visual brusco**
  💡 *Tip:* Usa un placeholder como “Página no autorizada” o una redirección con mensaje.

---

#### ✅ Validaciones

* Rutas sensibles no son accesibles sin login
* El usuario solo accede a lo que su rol permite
* Las rutas están protegidas tanto en navegación directa como en lazy load

---
