# 🧩 Laboratorio 3 – Gestión avanzada del estado con NGRX Signal + Classic Store

## 🧭 Objetivo del laboratorio

Este laboratorio amplía los conceptos trabajados en los dos anteriores, combinando `@ngrx/store` (versión clásica) con `@ngrx/signal-store`, para abordar la gestión del estado global, modularización de dominios y flujos altamente estructurados. El objetivo es dominar los patrones de escalado y segmentación del estado en aplicaciones Angular empresariales.

---

## 🔄 Enfoque

* Se reutilizará el dominio de plantas solares, pero ahora modelando múltiples dominios: instalaciones, alertas, usuarios.
* Se introducirá `SignalStore` para gestionar subdominios locales altamente reactivos.
* `Store` clásico con reducers + effects se utilizará para el estado compartido global.
* Se mantendrá una separación clara entre presentación, dominio y persistencia.

---

## 📅 Fases del laboratorio

### Fase 3.1 – Setup global con `@ngrx/store` y `@ngrx/effects`

Configuración inicial de store global, creación de slices de estado para `plants` y `alerts`, y carga desde la API usando effects.

### Fase 3.2 – Introducción de `SignalStore` para dominios locales

Creación de `PlantFilterStore` y `AlertViewStore` para gestionar lógica derivada reactiva y encapsulada por componente.

### Fase 3.3 – Modularización por features y lazy loading

Definición de módulos funcionales (`PlantModule`, `AlertModule`), configuración de `forFeature`, y uso de `StoreModule` en carga perezosa.

### Fase 3.4 – Coordinación entre stores: selección cruzada y composición

Sincronización entre stores globales y locales, derivación de estados combinados y comunicación de eventos desacoplados.

### Fase 3.5 – Persistencia selectiva y sincronización con localStorage

Implementación de `meta-reducers` para persistencia parcial del estado en almacenamiento local, y restauración en bootstrap.

### Fase 3.6 – Devtools, trazabilidad y testing de reducers/effects

Activación de Redux DevTools, uso de `provideMockStore` y `provideMockActions` para test unitario de estado y efectos.

### Fase 3.7 – Reflexión sobre escalado de estado en apps empresariales

Análisis de costes, beneficios y límites de NGRX. Comparación con ComponentStore y SignalStore en distintos niveles de complejidad.

---

## ✅ Validaciones

* El estado global es predictivo, serializable y trazable
* El estado local es reactivo, acotado
