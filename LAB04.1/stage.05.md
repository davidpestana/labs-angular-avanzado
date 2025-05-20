### ⚙️ Fase 4.5 – Mock de flujo OAuth2/OIDC y token JWT

#### 🎯 Objetivo

Simular un flujo de autenticación basado en OAuth2/OIDC usando tokens JWT. Aunque no hay backend real, el objetivo es preparar la app para trabajar con tokens y claims como en un entorno real.

---

#### 🗂️ Scaffolding

```
src/app/core/auth/
├── auth.service.ts           ← añade decodificación de JWT simulado
├── mock-token.ts             ← contiene un JWT de ejemplo (hardcoded)
```

---

#### 🪜 Pasos guiados

1. **Crear un token JWT simulado** (contenido codificado a mano):

```ts
export const MOCK_TOKEN = 
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.' +
  'eyJ1c2VybmFtZSI6ImFsaWNlIiwicm9sZSI6ImFkbWluIiwiZXhwIjoxODAwMDAwMDAwfQ.' +
  'dummy-signature';
```

2. **Añadir lógica para parsear el token en `AuthService`:**

```ts
function decodePayload(token: string): any {
  try {
    const payload = token.split('.')[1];
    return JSON.parse(atob(payload));
  } catch {
    return null;
  }
}
```

3. **Modificar `login()` para cargar desde el token:**

```ts
loginWithToken(token: string) {
  const payload = decodePayload(token);
  if (payload && payload.username && payload.role) {
    this.user.set({
      username: payload.username,
      role: payload.role,
      token
    });
  }
}
```

4. **Usar el token mock para iniciar sesión:**

```ts
this.auth.loginWithToken(MOCK_TOKEN);
```

---

#### 🎯 Retos y Tips

* **Verifica la expiración (`exp`) y fuerza logout si está vencido**
  💡 *Tip:* Añade un `isExpired()` que compare `payload.exp * 1000 < Date.now()` y usa `effect()` para auto-logout.

* **Guarda el token en `localStorage` y recupéralo al iniciar la app**
  💡 *Tip:* Usa `effect()` o ejecuta `loginWithToken(localStorage.getItem('token'))` en el constructor.

* **Simula distintos roles cambiando el payload del token**
  💡 *Tip:* Puedes usar una función `generateMockToken(username, role)` que construya dinámicamente el token base64.

---

#### ✅ Validaciones

* El token se decodifica y el usuario se carga correctamente
* El rol y usuario se extraen del payload
* La sesión puede expirar según el `exp` declarado

---
