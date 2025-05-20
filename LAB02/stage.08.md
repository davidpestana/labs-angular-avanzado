# ⚙️ Fase 2.8 – Reflexión y validación de arquitectura Redux

### 🎯 Objetivo

Finalizar el laboratorio reflexionando sobre la arquitectura basada en Redux (Component Store) implementada. Comparar sus beneficios y costes frente a la arquitectura basada en servicios RxJS del laboratorio anterior, y evaluar su idoneidad en distintos contextos.

---

### 🧠 Puntos de reflexión

1. **Organización del estado:**

   * ¿El estado centralizado ha ayudado a estructurar mejor la lógica?
   * ¿Se han reducido duplicidades o acoplamientos?

2. **Trazabilidad y debug:**

   * ¿Fue más fácil seguir el flujo de datos y acciones?
   * ¿Component Store permite depurar fácilmente qué cambió y por qué?

3. **Testabilidad y mantenimiento:**

   * ¿Las mutaciones puras permiten testear fácilmente la lógica?
   * ¿Separar efectos ayuda a mantener el código más robusto?

4. **Escalabilidad:**

   * ¿Se podría mantener esta arquitectura con 5-10 stores?
   * ¿En qué momento convendría migrar a `@ngrx/store` global?

5. **Comparativa con Lab01 (RxJS sin Redux):**

   * ¿Qué fue más sencillo de implementar y mantener?
   * ¿Cuál favorece más la separación de preocupaciones y la trazabilidad?

---

### ✅ Validaciones

* El alumno puede identificar los beneficios clave de Redux frente a servicios RxJS simples
* Se entienden los conceptos de `state`, `selector`, `updater`, `effect`
* Se valora la idoneidad de cada arquitectura según el tipo de aplicación

---

### 💬 Cierre

¿Este patrón te resulta cómodo para proyectos futuros? ¿Qué elementos adaptarías si el equipo fuera más grande o las vistas más complejas? ¿Qué parte del flujo ha resultado más clara o más repetitiva?
