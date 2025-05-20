# ⚙️ Fase 6.2 – Publicación continua en CDN o NPM

### 🎯 Objetivo

Automatizar la publicación de los bundles del widget Angular tras cada versión estable, desplegándolos directamente a un CDN o publicándolos como paquetes en NPM. Se busca lograr trazabilidad, distribución controlada y compatibilidad entre entornos.

---

### 🗂️ Estructura propuesta

```
.github/workflows/
├── release-widget.yml        ← pipeline de publicación

libs/widget/hello-widget/
├── dist/                     ← salida del bundle optimizado
├── package.json              ← metadatos de distribución
```

---

### 🪜 Pasos guiados

1. Configurar pipeline `release-widget.yml`:

```yaml
name: Release Widget
on:
  push:
    tags:
      - 'v*'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          registry-url: 'https://registry.npmjs.org/'
      - run: npm ci
      - run: npx nx build hello-widget --configuration=production
      - run: cd dist/libs/widget/hello-widget && npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

2. Alternativa: subir artefacto JS a un CDN estático

* Configurar upload a GitHub Pages, S3 o Netlify con un job adicional en el pipeline
* Publicar el archivo `.js` como recurso estático versionado

3. Validar que el paquete incluye:

* `main` o `exports` apuntando al bundle JS
* `peerDependencies` bien definidas si el consumidor también usa Angular
* `README.md`, `license`, `version` semántica correcta

4. Usar `changesets`, `standard-version` o scripts manuales para generar changelog y etiquetar versiones

---

### 🎯 Retos y Tips

* **Crear dos targets: `build` para test y `publish` para release**

  > 💡 *Tip:* Usa Nx target `outputPath` distinto para no pisar el build manual

* **Versionar automáticamente con `standard-version` o `changesets`**

  > 💡 *Tip:* Puedes usar PR semánticos con Conventional Commits y autogenerar el changelog

* **Desplegar automáticamente a Netlify o GitHub Pages para demo pública**

  > 💡 *Tip:* Usa artefacto de `dist/` y copia a un branch `gh-pages`

---

### ✅ Validaciones

* Cada tag genera un artefacto versionado y distribuido
* El widget puede ser instalado o embebido directamente desde un repositorio público
* La trazabilidad de versiones queda documentada y reproducible

---

### 💬 Reflexión

¿Es preferible NPM o CDN en tu caso? ¿Quién controla cuándo se publica una nueva versión? ¿Tu flujo de despliegue garantiza backwards compatibility?
