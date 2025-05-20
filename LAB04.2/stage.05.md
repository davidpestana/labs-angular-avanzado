# ⚙️ Fase 4.5 – Auditoría de seguridad básica y política de acceso de componentes

### 🎯 Objetivo

Evaluar y reforzar la seguridad de la aplicación desde el punto de vista del acceso a rutas, componentes y datos sensibles. Se definirá una política declarativa de visibilidad, se auditará el comportamiento según roles, y se aplicará lógica reactiva de ocultación/control basada en permisos.

---

### 🗂️ Scaffolding

```
src/app/auth/
├── auth.policy.ts           ← política centralizada de acceso
├── auth.audit.service.ts    ← trazabilidad de violaciones de acceso
├── role.directive.ts        ← directiva de control visual (ya implementada)

libs/plant/feature/          ← marcar componentes con metadata de acceso
```

---

### 🪜 Pasos guiados

1. Centraliza la política de acceso:

```ts
export const ACCESS_POLICY = {
  plants: ['admin', 'viewer'],
  alerts: ['admin'],
  settings: ['admin']
};
```

2. Crea una función de validación:

```ts
export function canAccess(resource: keyof typeof ACCESS_POLICY, role: string) {
  return ACCESS_POLICY[resource].includes(role);
}
```

3. Implementa `AuthAuditService`:

```ts
@Injectable({ providedIn: 'root' })
export class AuthAuditService {
  logAccessAttempt(resource: string, allowed: boolean) {
    console.log(`[Audit] ${allowed ? '✅' : '❌'} Acceso a ${resource}`);
  }
}
```

4. En componentes, usa metadata de acceso:

```ts
@Component({
  selector: 'app-sensitive-panel',
  templateUrl: './panel.html',
  data: { access: 'alerts' }
})
```

5. Comprobar acceso desde un wrapper:

```ts
if (!canAccess('alerts', user.role)) {
  authAuditService.logAccessAttempt('alerts', false);
  return null;
}
```

---

### 🎯 Retos y Tips

* **Ocultar elementos incluso en DOM si no se tienen permisos**

  > 💡 *Tip:* Usa `ngIfRole` o añade lógica al resolver rutas para no mostrar módulos directamente

* **Registrar intentos de acceso no permitidos por consola o servidor**

  > 💡 *Tip:* Usa `AuthAuditService` también desde `canActivate` o `canLoad`

* **Aplicar visibilidad condicional a elementos del layout (menús, tabs)**

  > 💡 *Tip:* Refactoriza menús con un `MenuItem[]` que filtre por política en tiempo de ejecución

---

### ✅ Validaciones

* Los intentos de acceso indebido son interceptados
* Solo los roles válidos ven los elementos que les corresponden
* Existe trazabilidad clara de los recursos accedidos o bloqueados

---

### 💬 Reflexión

¿Es suficiente el control desde el frontend? ¿Dónde encajarían mejor los enforcement fuertes (backend, gateway, token)? ¿Cómo harías escalable esta política si hubiera cientos de recursos?
