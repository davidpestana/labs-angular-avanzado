# ⚙️ Fase 6.5 – Versionado semántico, changelog y gestión de releases

### 🎯 Objetivo

Aplicar una estrategia de versionado semántico (semver) para los widgets distribuidos. Automatizar la generación de changelogs, la asignación de versiones, la creación de tags y el despliegue trazable con control de regresiones y comunicación clara a los consumidores.

---

### 🗂️ Estructura propuesta

```
.changeset/                      ← configuración de changesets
.github/workflows/
├── release.yml                 ← pipeline de publicación con semver
```

---

### 🪜 Pasos guiados

1. **Instalar y configurar `changesets`:**

```bash
npx changeset init
```

Esto crea el directorio `.changeset/` y una configuración inicial.

2. **Crear un changeset por cada cambio relevante:**

```bash
npx changeset
```

Selecciona el tipo de cambio (patch, minor, major) y describe brevemente la modificación.

3. **Generar versión y changelog automáticamente:**

```bash
npx changeset version
```

Esto actualiza `package.json` y genera el `CHANGELOG.md`.

4. **Publicar con `changeset publish` o desde CI:**

```bash
npx changeset publish
```

5. **Crear workflow `release.yml` para automatizar publicación tras cambios aprobados:**

```yaml
name: Release
on:
  push:
    branches: [main]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npx changeset version
      - run: git config user.name "CI Bot"
      - run: git config user.email "ci@example.com"
      - run: git add . && git commit -m "ci: version bump"
      - run: git push
      - run: npx changeset publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

### 🎯 Retos y Tips

* **Gestionar versiones específicas para múltiples widgets dentro del mismo repo**

  > 💡 *Tip:* Usa `changesets` con scoping (`@org/widget-a`) y actualiza cada uno por separado

* **Publicar versiones `canary` para testing previo**

  > 💡 *Tip:* Usa etiquetas tipo `v2.0.0-canary.1` para entornos beta antes del release final

* **Firmar tags y releases si usas GPG o GitHub Releases**

  > 💡 *Tip:* Añade pasos en el workflow para crear un `release` en GitHub con assets adjuntos

---

### ✅ Validaciones

* El changelog se genera automáticamente a partir de cambios declarados
* Cada versión publicada tiene un número único, semántico y trazable
* El pipeline CI garantiza consistencia entre código, changelog y paquete publicado

---

### 💬 Reflexión

¿Tu equipo necesita versiones formales para distribución? ¿Cómo comunicarías cambios sin romper integraciones? ¿Qué trade-offs introduces con versiones demasiado frecuentes o rígidas?
