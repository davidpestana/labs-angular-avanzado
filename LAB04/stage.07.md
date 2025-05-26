### ⚙️ Fase 4.7 – Reflexión final sobre seguridad frontend

#### 🎯 Objetivo

Cerrar el laboratorio con una reflexión técnica sobre las limitaciones del control de acceso desde el frontend y qué estrategias deben escalar hacia el backend o un proveedor de identidad real.

---

#### 💬 Puntos de reflexión

* **¿Qué parte de la autenticación debe confiar el frontend y cuál no?**

  > El frontend puede gestionar la experiencia de usuario (mostrar/ocultar, navegar, validar visualmente),
  > pero no debe asumir que un token no puede ser falsificado. Toda verificación crítica debe ocurrir en el backend.

* **¿Son seguras las rutas protegidas con guards?**

  > No. Los guards evitan navegación, pero el contenido del frontend ya puede estar cargado en memoria o visible por inspección. Solo son una barrera de UX.

* **¿Qué estrategia real usarías en producción?**

  > OAuth2/OIDC con tokens firmados, gestionados por un Identity Provider como Auth0, Keycloak o Azure AD.
  > Además, backend con validación de tokens y endpoints protegidos por rol o permiso.

* **¿Qué partes de este laboratorio se pueden portar directamente a producción?**

  > * El patrón de `AuthService` y decodificación del token
  > * Las directivas como `*ngIfRole`
  > * El uso de `guards` como barrera de navegación
  > * La arquitectura de roles basada en claims

* **¿Qué partes deberías eliminar o revisar?**

  > * Los tokens hardcoded
  > * Las claves o firmas inexistentes (en un token real, deberías validar firmas con JWKs)
  > * La simulación manual del `exp`

---

#### ✅ Validaciones finales

* El usuario no puede acceder a rutas o componentes sin permiso
* El sistema expira sesión de forma segura
* La lógica de roles se gestiona desde un lugar centralizado (AuthService)

---
