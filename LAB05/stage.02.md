# ⚙️ Fase 5.2 – Aislar el entorno con standalone y bootstrap manual

### 🎯 Objetivo

Desacoplar completamente el componente de cualquier app Angular. Se eliminarán dependencias implícitas de módulos globales o rutas, y se construirá un entorno de ejecución mínimo basado solo en el componente, providers necesarios y `bootstrapApplication()`.

---

### 🗂️ Scaffolding

```
libs/widget/hello-widget/
├── hello-widget.component.ts
├── hello-widget.bootstrap.ts     ← configuración de arranque manual
├── main.ts                       ← punto de entrada final
```

---

### 🪜 Pasos guiados

1. Extraer la lógica de bootstrap en su propio archivo:

```ts
export function setupHelloWidget() {
  return bootstrapApplication(HelloWidgetComponent, {
    providers: [provideCustomElements()]
  }).then(() => {
    customElements.define('hello-widget', HelloWidgetComponent as any);
  });
}
```

2. Usar `main.ts` solo para iniciar:

```ts
import { setupHelloWidget } from './hello-widget.bootstrap';
setupHelloWidget();
```

3. Asegurarse de no depender de ningún `AppModule`, `RouterModule` o imports globales.

4. Verificar que el componente puede ser ejecutado por sí solo, sin rutas, sin NgModules, sin navegación.

5. Opcional: añadir entorno de test o `host.html` que incluya únicamente el bundle generado.

---

### 🎯 Retos y Tips

* **Eliminar dependencias implícitas del entorno Angular**

  > 💡 *Tip:* Evita `RouterModule`, `AppModule`, `BrowserAnimationsModule` si no son necesarios

* **Configurar providers mínimos sin errores**

  > 💡 *Tip:* Usa `provideHttpClient()`, `provideAnimations()` solo si son realmente usados por el widget

* **Simular múltiples widgets independientes cargados en paralelo**

  > 💡 *Tip:* Crea una página con `<hello-widget name="A"></hello-widget>` y otra instancia con distintos atributos

---

### ✅ Validaciones

* El componente puede ejecutarse de forma aislada
* El bootstrap no depende de ningún módulo global
* Se puede cargar en paralelo con otras instancias del mismo tipo

---

### 💬 Reflexión

¿Esta forma de arranque es más fácil de mantener y portar? ¿Qué impacto tiene separar el `bootstrap` en la portabilidad del código? ¿En qué casos usarías este patrón incluso dentro de una app Angular?
