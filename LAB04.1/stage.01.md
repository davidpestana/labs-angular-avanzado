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

¿Dónde deberían vivir los datos de sesión en una app real? ¿Este enfoque es suficiente para garantizar una experiencia segura? ¿Qué camb
