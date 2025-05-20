# ⚙️ Fase 4.6 – Bundles, análisis de dependencias y presupuestos de compilación

### 🎯 Objetivo

Medir, controlar y optimizar el tamaño de los bundles de la aplicación. Analizar las dependencias utilizadas, detectar cargas innecesarias y definir límites (presupuestos) que eviten degradaciones en builds futuros.

---

### 🗂️ Scaffolding

```
angular.json                    ← configuración de budgets
apps/solar-monitor/
├── main.ts
├── polyfills.ts
├── ...                         ← punto de entrada para análisis

nx.json / workspace.json        ← análisis de dependencias
```

---

### 🪜 Pasos guiados

1. Ejecuta un build con stats para analizar:

```bash
ng build --configuration=production --stats-json
```

2. Visualiza el resultado con `webpack-bundle-analyzer`:

```bash
npm install -D webpack-bundle-analyzer
npx webpack-bundle-analyzer dist/apps/solar-monitor/browser/stats.json
```

3. Revisa dependencias innecesarias:

* Identifica paquetes grandes no utilizados (icons, charting, intl...)
* Valora el impacto de librerías de UI, lodash, moment, etc.

4. Define budgets en `angular.json`:

```json
"budgets": [
  {
    "type": "initial",
    "maximumWarning": "2mb",
    "maximumError": "3mb"
  },
  {
    "type": "anyComponentStyle",
    "maximumWarning": "6kb"
  }
]
```

5. Ejecuta `nx graph` para visualizar dependencias entre libs:

```bash
nx dep-graph
```

---

### 🎯 Retos y Tips

* **Reducir el tamaño del bundle inicial en al menos un 20%**

  > 💡 *Tip:* Aplica lazy loading real, elimina imports innecesarios en `app.module`, revisa qué incluye cada `feature` lib

* **Configurar budgets de forma razonable por app y por lib**

  > 💡 *Tip:* Usa distintos valores para `initial`, `lazy`, `styles`, y ajusta por entorno si usas múltiples targets

* **Detectar dependencias cruzadas o duplicadas entre libs**

  > 💡 *Tip:* Usa `nx lint` y `nx graph --focus=libX` para ver qué otras libs dependen de una dada

---

### ✅ Validaciones

* Se detectan y eliminan dependencias no críticas
* Los budgets están definidos y activan warning/error al superarlos
* Los bundles están organizados por dominio funcional y no incluyen redundancias

---

### 💬 Reflexión

¿Dónde está el verdadero peso de tu aplicación? ¿Cuáles de esas dependencias puedes evitar? ¿Qué parte podrías delegar al backend o cargar bajo demanda?
