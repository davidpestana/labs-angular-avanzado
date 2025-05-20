### ⚙️ Fase 4.6 – Logout y expiración de sesión

#### 🎯 Objetivo

Completar el ciclo de autenticación implementando la lógica de cierre de sesión manual y automática por expiración de token.

---

#### 🗂️ Scaffolding

```
src/app/core/auth/
├── auth.service.ts        ← ampliar logout y expiración
```

---

#### 🪜 Pasos guiados

1. **Extender `AuthService` para detectar expiración automática del token:**

```ts
private expirationTimer: any;

private scheduleTokenExpiration(exp: number) {
  const delay = exp * 1000 - Date.now();
  if (delay > 0) {
    this.expirationTimer = setTimeout(() => this.logout(), delay);
  }
}
```

2. **Detectar expiración al hacer login:**

```ts
loginWithToken(token: string) {
  const payload = decodePayload(token);
  if (payload?.username && payload?.role) {
    this.user.set({ username: payload.username, role: payload.role, token });
    if (payload.exp) this.scheduleTokenExpiration(payload.exp);
  }
}
```

3. **Extender `logout()` para limpiar `localStorage` y timers:**

```ts
logout() {
  this.user.set(null);
  localStorage.removeItem('token');
  clearTimeout(this.expirationTimer);
}
```

4. **Recuperar la sesión al iniciar la app si el token aún es válido:**

```ts
const saved = localStorage.getItem('token');
if (saved) this.loginWithToken(saved);
```

---

#### 🎯 Retos y Tips

* **Mostrar una alerta visual antes de expiración (últimos segundos)**
  💡 *Tip:* Usa otro `setTimeout()` para lanzar un aviso 10 segundos antes del vencimiento.

* **Emitir un evento o usar `effect()` para avisar al usuario de que ha sido desconectado**
  💡 *Tip:* Usa `effect(() => this.user(), { allowSignalWrites: true })` y muestra un snackbar si `user === null`.

* **Evitar reiniciar el temporizador si el mismo token ya está activo**
  💡 *Tip:* Compara `token` actual y solo reactiva el temporizador si es distinto.

---

#### ✅ Validaciones

* El token se elimina y el usuario se desloguea manual o automáticamente
* La expiración ocurre en base al `exp` real del JWT
* El comportamiento es predecible y respetuoso con la seguridad del usuario

---
