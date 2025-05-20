# ⚙️ Fase 5.5 – Deploy como widget consumible (npm, CDN o HTML)

### 🎯 Objetivo

Preparar el Web Component para su distribución y uso externo. Se abordarán métodos para empaquetar, versionar y publicar el componente como artefacto reutilizable, ya sea en forma de bundle JS para CDN o como paquete npm para apps cliente.

---

### 🗂️ Scaffolding

```
dist/hello-widget/                    ← bundle generado
libs/widget/hello-widget/
├── package.json                      ← configuración npm (opcional)
├── host.html                         ← demo básica
```

---

### 🪜 Pasos guiados

1. Generar un bundle optimizado:

```bash
ng build hello-widget --configuration=production
```

2. Verifica que el `main.js` generado es autocontenible y puede ser cargado vía `<script>`.

3. Crear una demo `host.html` para testeo externo:

```html
<html>
  <body>
    <hello-widget name="TMEIC"></hello-widget>
    <script src="./dist/hello-widget/main.js"></script>
  </body>
</html>
```

4. Opcional: configurar `outputHashing: none` en `angular.json` para evitar nombres de archivo con hash al hacer deploy directo.

5. Publicar el bundle:

* 🔁 CDN: subir el archivo `main.js` a un bucket S3, GitHub Pages, Netlify, etc.
* 📦 NPM: crear `package.json`, definir `exports`, y publicar con `npm publish` si está configurado.

6. Importar en otro entorno:

```html
<script src="https://cdn.example.com/hello-widget.js"></script>
<hello-widget name="Cliente"></hello-widget>
```

---

### 🎯 Retos y Tips

* **Crear un bundle único sin dependencias externas**

  > 💡 *Tip:* Usa `externalDependencies: []` y configura `standalone: true` para evitar Angular runtime externo

* **Probar el widget en una app React, CMS o dashboard ajeno**

  > 💡 *Tip:* Usa `<script>` global y manipula el DOM como cualquier otro custom element

* **Versionar y empaquetar correctamente para múltiples entornos**

  > 💡 *Tip:* Usa `exports` en `package.json` para exponer solo el JS final

---

### ✅ Validaciones

* El bundle funciona en cualquier página HTML o framework
* No requiere Angular instalado en el consumidor
* Puede ser cacheado, versionado y publicado como cualquier librería

---

### 💬 Reflexión

¿Es mejor publicar un widget como bundle JS o como lib npm? ¿Cómo se puede garantizar compatibilidad entre versiones? ¿Qué cambios requiere mantener backwards compatibility?
