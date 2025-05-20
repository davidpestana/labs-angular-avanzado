# ⚙️ Fase 5.1 – Exportar un componente Angular como Web Component

### 🎯 Objetivo

Transformar un componente Angular en un Web Component reutilizable, registrándolo como Custom Element mediante `createCustomElement` o `defineCustomElement` (Angular 14+). El objetivo es que funcione en cualquier entorno web sin necesidad del framework completo.

---

### 🗂️ Scaffolding

```
libs/widget/hello-widget/
├── hello-widget.component.ts   ← componente standalone
├── main.ts                     ← punto de entrada custom
├── package.json                ← configuración para empaquetado
```

---

### 🪜 Pasos guiados

1. Crear un componente standalone:

```ts
@Component({
  selector: 'hello-widget',
  standalone: true,
  template: `<div>Hello {{ name }}!</div>`,
  imports: [CommonModule]
})
export class HelloWidgetComponent {
  @Input() name = 'World';
}
```

2. Crear `main.ts` para exponer el componente:

```ts
import { bootstrapApplication } from '@angular/platform-browser';
import { provideCustomElements } from '@angular/elements';
import { HelloWidgetComponent } from './hello-widget.component';

bootstrapApplication(HelloWidgetComponent, {
  providers: [provideCustomElements()]
}).then(() => {
  customElements.define('hello-widget', HelloWidgetComponent as any);
});
```

3. Asegurarse de que se construye como aplicación o microbundle:

```json
// angular.json
targets: {
  build: {
    builder: '@angular-devkit/build-angular:application',
    options: {
      outputPath: 'dist/widget/hello-widget',
      main: 'libs/widget/hello-widget/main.ts'
    }
  }
}
```

4. Generar el build y obtener `hello-widget.js`:

```bash
ng build hello-widget
```

5. Probarlo en un HTML plano:

```html
<html>
  <body>
    <hello-widget name="TMEIC"></hello-widget>
    <script src="./hello-widget.js"></script>
  </body>
</html>
```

---

### 🎯 Retos y Tips

* **Pasar propiedades como `@Input()` y validarlas dinámicamente**

  > 💡 *Tip:* Usa `@Input()`s estrictos y valores por defecto para entornos no tipados

* **Publicar el script y probarlo en un CMS externo o JS vanilla**

  > 💡 *Tip:* Usa un `index.html` aislado con CDN local y sin dependencias Angular

* **Medir el tamaño del bundle generado y reducirlo**

  > 💡 *Tip:* Usa `ng build --configuration=production --stats-json` y `webpack-bundle-analyzer`

---

### ✅ Validaciones

* El componente es visible en HTML plano sin Angular extra
* Acepta valores externos como atributos HTML
* Su código fuente se empaqueta correctamente en un solo archivo JS

---

### 💬 Reflexión

¿Es esta técnica útil para compartir componentes entre proyectos? ¿Qué diferencia hay con una app Angular completa embebida? ¿Qué limitaciones ves al usar `@Input()`s en este contexto?
