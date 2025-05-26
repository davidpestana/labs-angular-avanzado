### ⚙️ Fase 4.3 – Visibilidad condicional en componentes

#### 🎯 Objetivo

Controlar la visibilidad de elementos en la interfaz según el estado de autenticación y el rol del usuario, directamente desde el componente sin necesidad de navegación.

---

#### 🗂️ Scaffolding

```
src/app/pages/dashboard/
├── dashboard.component.ts
├── dashboard.component.html

src/app/core/auth/
├── auth.service.ts (extendido)
```

---

#### 🪜 Pasos guiados

1. **Ampliar `AuthService` con métodos de consulta rápida:**

```ts
hasAnyRole(...roles: string[]): boolean {
  const user = this.user();
  return !!user && roles.includes(user.role);
}
```

2. **Mostrar elementos condicionalmente en el template:**

```html
<!-- Solo para admin -->
<div *ngIf="auth.hasRole('admin')">
  Acciones administrativas
</div>

<!-- Para admin o editor -->
<div *ngIf="auth.hasAnyRole('admin', 'editor')">
  Acceso a edición de contenido
</div>
```

3. **Alternativa con signals y `*ngIf`:**

```ts
canEdit = computed(() => this.auth.currentUser()?.role === 'editor');
```

```html
<div *ngIf="canEdit()">Contenido editable</div>
```

---

#### 🎯 Retos y Tips

* **Centraliza las comprobaciones en el servicio**
  💡 *Tip:* Evita repetir lógica en cada componente. Expón métodos `isAdmin()`, `canViewDashboard()`, etc.

* **Mostrar alertas cuando el usuario no tiene permiso**
  💡 *Tip:* Usa una variable `!auth.hasAnyRole(...)` para mostrar un `<p>No tienes permiso para ver esta sección</p>` en vez de ocultarlo completamente.

* **Combinar visibilidad con animaciones o transiciones**
  💡 *Tip:* Usa `[@fade]` o `*ngIf + ngClass` para una transición suave al mostrar u ocultar contenido.

---

#### ✅ Validaciones

* Los bloques del DOM se muestran u ocultan según el rol
* No hay errores de acceso cuando no hay usuario
* El componente permanece usable para distintos perfiles de usuario

---
