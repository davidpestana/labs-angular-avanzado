# ⚙️ Fase 4.4 – Migración de features a librerías desacopladas (Nx/lib)

### 🎯 Objetivo

Separar los dominios funcionales (`plants`, `alerts`, etc.) en librerías independientes usando Nx, reorganizando la arquitectura para mejorar la mantenibilidad, trazabilidad y reusabilidad. Se introducen restricciones de dependencia y un control claro de los límites entre capas.

---

### 🗂️ Scaffolding

```
libs/
├── plant/feature/           ← lógica de presentación + páginas
├── plant/data-access/       ← store, efectos, selectores
├── alert/feature/
├── alert/data-access/

apps/solar-monitor/          ← shell principal
```

---

### 🪜 Pasos guiados

1. Inicializa Nx (si no está):

```bash
npx nx init
```

2. Genera librerías por dominio y responsabilidad:

```bash
nx g @nx/angular:lib plant-feature --directory=plant --standalone
nx g @nx/angular:lib plant-data-access --directory=plant
```

3. Mueve los archivos correspondientes:

* Páginas y componentes → `plant/feature`
* Estado, store, actions → `plant/data-access`

4. Reexporta desde `index.ts` de cada lib:

```ts
export * from './lib/plant-list.component';
```

5. Usa las libs desde la app:

```ts
import { PlantListComponent } from '@myorg/plant/feature';
```

6. Configura restricciones y límites de dependencia:

```ts
// nx.json
'tags': ['scope:plant', 'type:feature']
```

Usa `nx lint` para forzar boundaries entre libs (`enforceModuleBoundaries`)

---

### 🎯 Retos y Tips

* **Crear una lib `ui-kit` para componentes comunes compartidos**

  > 💡 *Tip:* Agrupa botones, tablas, paneles, etc., y aplica tag `type:ui`

* **Añadir tags a todas las libs y prevenir dependencias cruzadas**

  > 💡 *Tip:* Usa `"implicitDependencies": false` y `dependencyConstraints` en `nx.json`

* **Usar `dep-graph` de Nx para visualizar la arquitectura**

  > 💡 *Tip:* Ejecuta `nx graph` o `nx dep-graph --file=out.html`

---

### ✅ Validaciones

* Cada dominio está separado en al menos dos libs (`feature`, `data-access`)
* No hay dependencias circulares entre librerías
* La aplicación sigue funcionando y compila con imports desde `@org/...`

---

### 💬 Reflexión

¿Esta modularización aporta más claridad o más fricción? ¿Qué parte fue más costosa de mover? ¿Se podría aplicar este enfoque sin Nx?
