# ⚙️ Fase 5.4 – Encapsulamiento de estilos y uso de Shadow DOM

### 🎯 Objetivo

Aislar visualmente el componente expuesto como Web Component usando Shadow DOM. Se busca evitar colisiones de estilos, asegurar consistencia visual en cualquier entorno y controlar la herencia de CSS externa.

---

### 🗂️ Scaffolding

```
libs/widget/hello-widget/
├── hello-widget.component.ts         ← con encapsulación explícita
├── styles/hello-widget.styles.css    ← hoja de estilos interna
```

---

### 🪜 Pasos guiados

1. Activar encapsulación con Shadow DOM en el componente:

```ts
@Component({
  selector: 'hello-widget',
  standalone: true,
  encapsulation: ViewEncapsulation.ShadowDom,
  template: `<div class="container">Hola {{ name }}!</div>`,
  styleUrls: ['./styles/hello-widget.styles.css']
})
export class HelloWidgetComponent {
  @Input() name = 'World';
}
```

2. Asegurar que los estilos aplican sólo al widget:

```css
.container {
  font-family: 'Segoe UI', sans-serif;
  color: #4e4e4e;
  border: 1px solid #ccc;
  padding: 1rem;
  border-radius: 4px;
}
```

3. Verificar que no se heredan estilos externos del entorno HTML:

* En un archivo `host.html`, aplica un `body { color: red; font-size: 30px }`
* El contenido del widget no debe verse afectado

4. Probar con múltiples widgets en la misma página, con estilos distintos:

```html
<hello-widget name="A"></hello-widget>
<hello-widget name="B"></hello-widget>
```

---

### 🎯 Retos y Tips

* **Probar el widget dentro de una app con estilos globales agresivos (Bootstrap, Tailwind)**

  > 💡 *Tip:* Usa `ViewEncapsulation.ShadowDom` y verifica aislamiento completo

* **Incluir fuentes o assets sin romper el aislamiento**

  > 💡 *Tip:* Usa `@import` en el `.css` o `<link rel="stylesheet">` dinámico desde el `ShadowRoot`

* **Aplicar estilos dinámicos desde atributos HTML o props JS**

  > 💡 *Tip:* Usa `style` bindings dentro del template (`[style.backgroundColor]`) para personalización sin romper encapsulación

---

### ✅ Validaciones

* Los estilos del widget no se ven afectados por CSS externo
* Varias instancias del widget pueden coexistir con estilos independientes
* Los assets internos (fuentes, colores) se mantienen consistentes

---

### 💬 Reflexión

¿El Shadow DOM mejora la portabilidad del widget? ¿Qué problemas puede provocar (debug, SSR, print)? ¿Dónde sería imprescindible y dónde prescindible?
