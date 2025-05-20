# ⚙️ Fase 6.1 – Automatización de build y test con Nx y GitHub Actions

### 🎯 Objetivo

Automatizar el proceso de construcción, testing y validación del componente distribuido (widget) usando Nx en combinación con GitHub Actions. Se busca garantizar builds reproducibles, control de regresiones y feedback continuo en pull requests o pushes a rama principal.

---

### 🗂️ Estructura propuesta

```
.github/workflows/
├── ci-widget.yml                ← pipeline de build y test

libs/widget/hello-widget/
├── *.spec.ts                    ← tests unitarios
├── e2e/                         ← pruebas de integración o visuales
```

---

### 🪜 Pasos guiados

1. Crear el workflow `ci-widget.yml`:

```yaml
name: Build and Test Widget
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npx nx run hello-widget:build
      - run: npx nx test hello-widget
```

2. Asegurarse de que existen tests unitarios mínimos (`.spec.ts`) en el componente:

```ts
it('debe renderizar con nombre', () => {
  const fixture = TestBed.createComponent(HelloWidgetComponent);
  fixture.componentInstance.name = 'Test';
  fixture.detectChanges();
  expect(fixture.nativeElement.textContent).toContain('Hola Test');
});
```

3. Opcional: añadir tests visuales o de integración en un entorno externo (usando `playwright`, `cypress`, o `puppeteer` con el widget embebido).

4. Verifica que los errores en tests o build fallan el PR automáticamente.

---

### 🎯 Retos y Tips

* **Separar jobs por etapa (`build`, `test`, `lint`) para mayor claridad**

  > 💡 *Tip:* Usa `needs:` y nombres de job distintos para encadenar stages en GitHub Actions

* **Usar `nx affected:test` para optimizar en PRs**

  > 💡 *Tip:* Evalúa solo los proyectos afectados por los cambios con `npx nx affected:test` en lugar de test completo

* **Añadir cobertura y badge de estado en README**

  > 💡 *Tip:* Usa `codecov` o `coveralls` para subir el coverage y `shields.io` para añadir un badge dinámico

---

### ✅ Validaciones

* Cada PR ejecuta build y test del widget automáticamente
* Los errores son visibles directamente en la interfaz de GitHub
* La ejecución es reproducible y desacoplada del entorno local

---

### 💬 Reflexión

¿La automatización mejora tu seguridad al integrar cambios? ¿Qué parte del flujo se rompe si no hay tests mínimos? ¿Este pipeline se adapta a múltiples widgets o debería ser más granular?
