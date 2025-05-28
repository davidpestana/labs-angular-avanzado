# 🧩 Manual: Integración de OAuth 2.0 + OpenID Connect en Angular

Este documento explica cómo implementar autenticación OAuth 2.0 con OpenID Connect (OIDC) en una aplicación Angular utilizando cualquier proveedor compatible con OIDC (como Google, Auth0, Keycloak, Azure AD, etc.). En el ejemplo se aplica Google como caso ilustrativo.

---

## 🎯 Objetivo

Permitir que los usuarios se autentiquen en una SPA Angular utilizando un proveedor externo de identidad (OAuth/OIDC), siguiendo el flujo de autorización seguro Authorization Code + PKCE.

---

## 🔧 Paso 1: Registrar aplicación en el proveedor OAuth

Cualquier proveedor de OAuth 2.0 + OIDC requiere lo siguiente:

- **Issuer (URL del proveedor)**: Ej. `https://accounts.google.com`, `https://login.microsoftonline.com/...`, `https://your-keycloak-server/realms/your-realm`
- **Client ID**: identificador generado al registrar tu aplicación.
- **Redirect URI**: redirección post-login (ej. `http://localhost:4200/index.html`)

### Ejemplo: Google

1. Accede a [Google Cloud Console](https://console.cloud.google.com/)
2. Crea un nuevo proyecto.
3. En "APIs y servicios > Credenciales", crea un **ID de cliente de OAuth 2.0**:
   - Tipo: Aplicación web
   - URI de redirección: `http://localhost:4200/index.html`
   - Origen autorizado: `http://localhost:4200`
4. Copia el `clientId` generado.

---

## 📦 Paso 2: Instalar librería cliente

```bash
npm install angular-oauth2-oidc
```

---

## ⚙️ Paso 3: Configurar OAuth en Angular

### 📄 `auth.config.ts`

```ts
import { AuthConfig } from 'angular-oauth2-oidc';

export const authConfig: AuthConfig = {
  issuer: 'https://accounts.google.com', // Sustituir por el issuer de tu proveedor
  clientId: 'TU_CLIENT_ID.apps.googleusercontent.com',
  redirectUri: window.location.origin + '/index.html',
  responseType: 'code',
  scope: 'openid profile email',
  strictDiscoveryDocumentValidation: false,
  showDebugInformation: true,
  clearHashAfterLogin: false
};
```

---

## 🧠 Paso 4: Inicializar OAuthService

### 📄 `app.component.ts`

```ts
import { Component } from '@angular/core';
import { OAuthService } from 'angular-oauth2-oidc';
import { authConfig } from './auth.config';

@Component({
  selector: 'app-root',
  template: `
    <button (click)="login()">Login</button>
    <button (click)="logout()">Logout</button>
    <pre *ngIf="userProfile">{{ userProfile | json }}</pre>
  `
})
export class AppComponent {
  userProfile: any;

  constructor(private oauthService: OAuthService) {
    this.configure();
  }

  private async configure() {
    this.oauthService.configure(authConfig);
    await this.oauthService.loadDiscoveryDocumentAndTryLogin();

    if (this.oauthService.hasValidAccessToken()) {
      this.userProfile = this.oauthService.getIdentityClaims();
    }
  }

  login() {
    this.oauthService.initCodeFlow();
  }

  logout() {
    this.oauthService.logOut();
  }
}
```

---

## 🔐 Paso 5: Permitir envío de tokens a recursos protegidos

### 📄 `app.module.ts`

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { OAuthModule } from 'angular-oauth2-oidc';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    OAuthModule.forRoot({
      resourceServer: {
        allowedUrls: ['https://www.googleapis.com'],
        sendAccessToken: true
      }
    })
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

---

## ✅ Validaciones y pruebas

- Iniciar sesión → redirección al proveedor → login → regreso con token
- Inspeccionar `access_token` y `id_token` (usando `oauthService.getAccessToken()`)
- Las llamadas a APIs protegidas deben recibir el token en el encabezado Authorization
- Logout invalida la sesión local

---

## 🔐 Consideraciones de seguridad

- Usar `responseType: 'code'` + PKCE (ya soportado por la librería)
- Preferir `sessionStorage` o manejo de tokens en memoria
- HTTPS obligatorio en producción
- Validar la firma y expiración de tokens si es backend

---

## 🧪 Herramientas útiles

- [jwt.io](https://jwt.io) para inspeccionar tokens
- [OAuth 2.0 Playground](https://developers.google.com/oauthplayground)
- Paneles del proveedor (Google, Azure, Auth0, Keycloak...)

---

Este manual proporciona una guía general para integrar cualquier proveedor de identidad compatible con OAuth 2.0/OIDC en Angular. El ejemplo mostrado utiliza Google como caso práctico, pero la estructura es aplicable a cualquier otro proveedor compatible.
