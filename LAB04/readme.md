# 🛡️ Laboratorio 4 – Seguridad avanzada

## 🧭 Objetivo del laboratorio

Integrar un flujo completo de autenticación y autorización en una aplicación Angular, incluyendo protección de rutas, control de visibilidad por roles, y la base para una sesión segura. Se utilizará OAuth2/OpenID Connect como modelo de autenticación estándar.

> ⚠️ **Este laboratorio es anexo y aislado del resto del proyecto principal.** No reutiliza el mismo repositorio, ya que su enfoque requiere integrar múltiples proveedores de autenticación, tokens JWT y simulaciones de seguridad que no deben mezclarse con el entorno del proyecto iterativo anterior.

---

## 🎯 Enfoque

* Protección de rutas con `canActivate` y `canLoad`
* Gestión de roles y visibilidad condicional con directivas
* Simulación de sesión autenticada con datos de usuario
* Integración (mock) de flujo OAuth2/OIDC con token y claims

---

## 📅 Fases del laboratorio

### Fase 4.1 – Simulación de sesión y usuario autenticado

### Fase 4.2 – Protección de rutas con guards y roles

### Fase 4.3 – Visibilidad condicional en componentes

### Fase 4.4 – Directiva `*ngIfRole` reutilizable

### Fase 4.5 – Mock de flujo OAuth2/OIDC y token JWT

### Fase 4.6 – Logout y expiración de sesión

### Fase 4.7 – Reflexión final sobre seguridad frontend

---

## ✅ Validaciones

* Las rutas sensibles están protegidas según rol
* Los componentes muestran u ocultan contenido según permisos
* La sesión simula un usuario real y puede ser invalidada
* El sistema es ampliable a un flujo real de autenticación externa

---

## 💬 Reflexión

¿Hasta qué punto puedes confiar en la seguridad gestionada desde frontend? ¿Qué lógica deberías reforzar desde backend? ¿Qué parte del flujo es reutilizable en producción y qué partes solo sirven como mock?

---

# ⚙️ Fase 4.1 – Simulación de sesión y usuario autenticado

### 🎯 Objetivo

Establecer una base para gestionar una sesión de usuario simulada en la aplicación Angular. Se implementará un `AuthService` que gestione el estado de autenticación y proporcione información del usuario, incluyendo su rol. Esta fase no requiere backend: todo es mock.

---

### 🗂️ Scaffolding

```
src/app/core/
├── auth/
│   ├── auth.service.ts           ← estado de sesión y usuario
│   └── user.model.ts             ← interfaz de usuario simulado

src/app/app.component.ts         ← lógica de login/logout
src/app/app.component.html       ← controles de sesión
```

---

### 🪜 Pasos guiados

1. Crear modelo de usuario:

```ts
export interface User {
  username: string;
  role: 'admin' | 'editor' | 'viewer';
  token: string;
}
```

2. Implementar `AuthService` básico:

```ts
@Injectable({ providedIn: 'root' })
export class AuthService {
  private user = signal<User | null>(null);

  login(username: string, role: User['role']) {
    this.user.set({ username, role, token: 'mock-token' });
  }

  logout() {
    this.user.set(null);
  }

  isAuthenticated = computed(() => this.user() !== null);
  currentUser = computed(() => this.user());
  hasRole = (role: string) => this.user()?.role === role;
}
```

3. Añadir controles en `AppComponent`:

```html
<div *ngIf="!(auth.isAuthenticated() as boolean)">
  <button (click)="auth.login('alice', 'admin')">Login como Admin</button>
  <button (click)="auth.login('bob', 'viewer')">Login como Viewer</button>
</div>

<div *ngIf="auth.isAuthenticated()">
  Bienvenido, {{ auth.currentUser()?.username }} ({{ auth.currentUser()?.role }})
  <button (click)="auth.logout()">Logout</button>
</div>
```

---

### 🎯 Retos y Tips

* **Persistir la sesión en `localStorage`**

  > 💡 *Tip:* Usa `effect()` en el servicio para guardar y recuperar el usuario con `JSON.stringify`/`parse`

* **Simular expiración de sesión tras X segundos**

  > 💡 *Tip:* Usa `setTimeout(() => this.logout(), 10000)` tras login para simular expiración automática

* **Exponer `isAuthenticated()` como signal y como booleano en el template**

  > 💡 *Tip:* Usa el operador `()` en el HTML o combina con `toSignal()` si lo necesitas observable

---

### ✅ Validaciones

* El usuario puede iniciar y cerrar sesión
* La información del usuario está disponible desde cualquier componente
* El rol se interpreta correctamente como parte del estado

---

### 💬 Reflexión

¿Dónde deberían vivir los datos de sesión en una app real? ¿Este enfoque es suficiente para garantizar una experiencia segura? ¿Qué cambiarías si estuvieras en un entorno con múltiples servicios consumidores?
