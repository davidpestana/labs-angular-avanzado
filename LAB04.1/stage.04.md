### ⚙️ Fase 4.4 – Directiva `*ngIfRole` reutilizable

#### 🎯 Objetivo

Crear una directiva estructural personalizada que simplifique la visibilidad condicional según rol de usuario. Facilita la lectura del código y permite reutilizar lógica de acceso sin repetir condiciones.

---

#### 🗂️ Scaffolding

```
src/app/shared/directives/
├── if-role.directive.ts      ← directiva `*ngIfRole`

src/app/shared/
├── shared.module.ts          ← exporta la directiva

src/app/pages/                ← uso en templates
```

---

#### 🪜 Pasos guiados

1. **Crear la directiva `IfRoleDirective`:**

```ts
@Directive({
  selector: '[ngIfRole]',
  standalone: true
})
export class IfRoleDirective {
  private hasView = false;

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef,
    private auth: AuthService
  ) {}

  @Input()
  set ngIfRole(expectedRoles: string | string[]) {
    const roles = Array.isArray(expectedRoles) ? expectedRoles : [expectedRoles];
    const currentRole = this.auth.currentUser()?.role;

    if (currentRole && roles.includes(currentRole)) {
      if (!this.hasView) {
        this.viewContainer.createEmbeddedView(this.templateRef);
        this.hasView = true;
      }
    } else {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }
}
```

2. **Usar la directiva en un componente:**

```html
<div *ngIfRole="'admin'">Solo visible para administradores</div>

<div *ngIfRole="['admin', 'editor']">Visible para admin o editor</div>
```

3. **(Opcional) Añadir la directiva a `shared.module.ts` si no es standalone:**

```ts
@NgModule({
  declarations: [IfRoleDirective],
  exports: [IfRoleDirective]
})
export class SharedModule {}
```

---

#### 🎯 Retos y Tips

* **Convertir la directiva en standalone**
  💡 *Tip:* Ya lo es si tiene `standalone: true`, así puedes usarla directamente en `imports: []` del componente donde se necesita.

* **Añadir negación (`*ngIfNotRole`)**
  💡 *Tip:* Crea una segunda directiva `ngIfNotRole` o una propiedad `negate = true` como input adicional.

* **Mostrar loading si no hay sesión cargada**
  💡 *Tip:* Puedes extender la lógica para esperar a que `auth.currentUser()` esté inicializado (por ejemplo, desde localStorage).

---

#### ✅ Validaciones

* Los elementos se renderizan correctamente según el rol
* La directiva puede usarse en múltiples componentes
* El código es más legible que con `*ngIf="auth.hasRole(...)"`

---
