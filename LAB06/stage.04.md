# ⚙️ Fase 6.4 – Auditoría de tamaño, calidad y seguridad

### 🎯 Objetivo

Analizar el widget desde un punto de vista técnico integral: tamaño del bundle, cobertura de tests, calidad del código y dependencias vulnerables. Esta fase busca prevenir problemas en producción y asegurar estándares sostenibles para distribución pública.

---

### 🗂️ Estructura propuesta

```
dist/hello-widget/                   ← bundle final a auditar
coverage/                           ← informes de cobertura
.github/workflows/
├── audit.yml                       ← CI para auditoría técnica
```

---

### 🪜 Pasos guiados

1. **Medir tamaño del bundle generado:**

```bash
ng build hello-widget --configuration=production --stats-json
npx webpack-bundle-analyzer dist/hello-widget/browser/stats.json
```

2. **Evaluar cobertura de código:**

```bash
npx nx test hello-widget --code-coverage
```

Verifica que `coverage/` genera un informe con porcentaje mínimo aceptable (80% recomendado).

3. **Ejecutar análisis estático con ESLint:**

```bash
npx nx lint hello-widget
```

4. **Auditar dependencias vulnerables:**

```bash
npm audit --omit=dev
```

5. **Crear workflow `audit.yml`:**

```yaml
name: Audit and Quality Check
on: [pull_request]

jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npx nx lint hello-widget
      - run: npx nx test hello-widget --code-coverage
      - run: npm audit --omit=dev
```

---

### 🎯 Retos y Tips

* **Establecer un presupuesto de tamaño del bundle**

  > 💡 *Tip:* Usa `"budgets"` en `angular.json` para limitar el peso final del JS

* **Automatizar envío de cobertura a Codecov o Coveralls**

  > 💡 *Tip:* Integra un badge en el README y visualiza cambios por PR

* **Revisar licencias de dependencias si el widget se publica públicamente**

  > 💡 *Tip:* Usa `license-checker` o `oss-review-toolkit`

---

### ✅ Validaciones

* Bundle analizado con stats visuales y límites definidos
* Tests con cobertura y linting ejecutados automáticamente
* Dependencias vulnerables detectadas antes del despliegue

---

### 💬 Reflexión

¿Es sostenible este nivel de control para tu equipo? ¿Qué métricas te parecen críticas (peso, cobertura, bugs, licencias)? ¿Dónde está el equilibrio entre seguridad, velocidad y complejidad del flujo?
