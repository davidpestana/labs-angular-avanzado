/*
⚙️ Fase 5.1 – Exportar un componente Angular como Web Component
*/

// ─────────────────────────────────────────────
// 🎯 Objetivo
// Transformar un componente Angular en un Web Component reutilizable,
// registrándolo como Custom Element. El objetivo es que funcione en
// cualquier entorno web sin necesidad del framework completo.

// ─────────────────────────────────────────────
// 🗂️ Estructura: libs/widget/hello-widget/
// ├── hello-widget.component.ts   ← componente standalone
// ├── main.ts                     ← punto de entrada custom
// ├── index.html                  ← prueba aislada
// ├── angular.json                ← configuración build

// ─────────────────────────────────────────────
// 📁 hello-widget.component.ts
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-hello-widget',
  standalone: true,
  imports: [CommonModule],
  template: `<div>Hello {{ name }}!</div>`
})
export class HelloWidgetComponent {
  @Input() name = 'World';
}

// ─────────────────────────────────────────────
// 📁 main.ts
import { bootstrapApplication } from '@angular/platform-browser';
import { createCustomElement } from '@angular/elements';
import { importProvidersFrom } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HelloWidgetComponent } from './hello-widget.component';

bootstrapApplication(HelloWidgetComponent, {
  providers: [importProvidersFrom(BrowserModule)]
}).then(appRef => {
  const injector = appRef.injector;
  const element = createCustomElement(HelloWidgetComponent, { injector });
  customElements.define('app-hello-widget', element);
});

// ─────────────────────────────────────────────
// 📁 angular.json (fragmento relevante)
"projects": {
  "hello-widget": {
    "projectType": "application",
    "root": "libs/widget/hello-widget",
    "sourceRoot": "libs/widget/hello-widget",
    "architect": {
      "build": {
        "builder": "@angular-devkit/build-angular:application",
        "options": {
          "outputPath": "dist/widget/hello-widget",
          "index": "libs/widget/hello-widget/index.html",
          "main": "libs/widget/hello-widget/main.ts",
          "tsConfig": "libs/widget/hello-widget/tsconfig.app.json",
          "polyfills": [],
          "scripts": [],
          "styles": [],
          "assets": []
        }
      }
    }
  }
}

// ─────────────────────────────────────────────
// 📄 index.html (de prueba aislada)
<!DOCTYPE html>
<html>
  <head><title>Hello Widget</title></head>
  <body>
    <app-hello-widget name="Angular"></app-hello-widget>
    <script src="./main.js"></script>
  </body>
</html>

// ─────────────────────────────────────────────
// ✅ Validaciones:
// - El componente es visible en HTML plano sin Angular extra
// - Acepta valores externos como atributos HTML
// - Se empaqueta correctamente en un solo archivo JS

// ─────────────────────────────────────────────
// 💬 Reflexión:
// ¿Es esta técnica útil para compartir componentes entre proyectos?
// ¿Qué limitaciones ves al usar @Input()s en este contexto?

