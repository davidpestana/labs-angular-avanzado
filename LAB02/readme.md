# 🔁 Laboratorio 2 – Redux y Patrones Container/Presentational (Proyecto: Solar Monitor Redux)

## 🧭 Objetivo del laboratorio

Introducir la arquitectura unidireccional basada en Redux para separar la lógica de negocio y el estado de la presentación. Se aplicará esta arquitectura en el mismo dominio de gestión solar, ahora reutilizando parte de la API y mejorando la claridad, testabilidad y escalabilidad del frontend.

---

## 🧩 Fases del laboratorio

### Fase 2.1 – Bootstrap Redux desde arquitectura RxJS (Iteración sobre Lab01)

Establecer un punto de partida sobre la lógica existente usando `ComponentStore` para encapsular el estado y efectos. Refactor progresivo desde la lógica con `BehaviorSubject`.

### Fase 2.2 – Introducción del patrón Redux manual

Implementar una store simplificada basada en `BehaviorSubject` para modelar flujo Redux sin librerías externas. Introducción de acciones, reducers y efectos de forma manual.

### Fase 2.3 – Refactor a Component Store de NGRX

Reemplazar la implementación manual por `@ngrx/component-store` con `updater`, `selector` y `effect`, con énfasis en estados predecibles.

### Fase 2.4 – Comunicación entre componentes desacoplados

Crear múltiples contenedores que compartan estado. Enviar acciones y reacciones usando flujos unidireccionales controlados.

### Fase 2.5 – Gestión de side-effects y separación de comandos

Extraer operaciones externas (API, logs, navegación) hacia efectos y servicios secundarios. Implementar `tapResponse` y `concatLatestFrom`.

### Fase 2.6 – Refactor de servicios hacia acciones explícitas

Eliminar llamadas directas al servicio API desde los componentes, delegando la lógica completamente en acciones y efectos.

### Fase 2.7 – Gestión avanzada: filtros, estados derivados y validación

Diseñar slices del estado especializados, selectores compuestos, y lógica avanzada para filtros dinámicos, conteos o validaciones reactivas.

### Fase 2.8 – Reflexión y validación de arquitectura Redux

Analizar los costes y beneficios de Redux. Comparar con la arquitectura basada en servicios observables del laboratorio anterior.

---

## 🔧 Contenido funcional

* Componente presentacional `PlantListComponent` y contenedor `PlantListContainer`
* Acciones: `loadPlants`, `setFilter`, `setSelectedPlant`
* Estado: `{ plants: Plant[], selectedId: string | null, filter: string }`
* Component Store con `updater`, `effect`, `selector`
* Comunicación de eventos con `@Output()` y `dispatch`

---

## ✅ Validaciones

* Todos los estados están centralizados y reactivos
* Los presentacionales no contienen lógica de negocio
* Se ha reducido el acoplamiento y mejorado la trazabilidad de flujos

---

## 💬 Reflexión

¿Es Redux adecuado para todas las apps? ¿En qué momento empieza a aportar más valor que coste? ¿Cómo se puede modularizar esta arquitectura cuando escale a múltiples dominios funcionales?
