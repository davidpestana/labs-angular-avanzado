# ⚙️ Fase 6.3 – Test de compatibilidad en contexto externo (React, CMS, vanilla)

### 🎯 Objetivo

Validar que el widget publicado funciona correctamente en distintos entornos externos: frameworks como React o Vue, webs estáticas con JS plano, o entornos CMS. Se automatizan pruebas funcionales o visuales para asegurar integración cruzada.

---

### 🗂️ Estructura propuesta

```
integrations/
├── react-app/                ← proyecto React con consumo del widget
├── vanilla-html/            ← prueba con JS puro y HTML
├── cms-snippet.html         ← plantilla CMS externa (ej. WordPress)

.github/workflows/
├── compat-test.yml          ← pipeline para testing en contexto externo
```

---

### 🪜 Pasos guiados

1. Crear una app React de prueba:

```bash
npx create-react-app react-app
```

2. Consumir el widget desde React:

```tsx
useEffect(() => {
  const widget = document.querySelector('hello-widget');
  widget.setAttribute('name', 'React User');
  widget.addEventListener('greeted', e => console.log(e.detail));
}, []);
```

3. Crear un test HTML plano con `hello-widget.js` embebido:

```html
<script src="https://cdn.example.com/hello-widget.js"></script>
<hello-widget name="Prueba"></hello-widget>
```

4. Automatizar pruebas visuales con Playwright o Cypress:

```bash
npx playwright install
npx playwright test integrations/vanilla-html/test.spec.ts
```

5. Añadir `compat-test.yml` para CI:

```yaml
name: Compatibility Tests
on: [workflow_dispatch]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npm run compat:test
```

---

### 🎯 Retos y Tips

* **Detectar problemas en atributos HTML y eventos personalizados**

  > 💡 *Tip:* Usa `MutationObserver` en el test para validar DOM dinámico

* **Verificar múltiples instancias en React o Vue**

  > 💡 *Tip:* Crea varios widgets en una misma vista y verifica aislamiento visual y lógico

* **Simular errores de carga o dependencia no resuelta**

  > 💡 *Tip:* Rompe un script intencionadamente y analiza si el widget falla con fallback

---

### ✅ Validaciones

* El widget funciona sin errores en React, JS puro y CMS
* Los eventos se reciben correctamente en entornos no Angular
* Las pruebas externas se automatizan y validan integración real

---

### 💬 Reflexión

¿Hasta qué punto puedes garantizar que un widget funcionará fuera de Angular? ¿Qué parte del contrato con el consumidor externo es crítico definir (inputs, outputs, estilos, errores)?
