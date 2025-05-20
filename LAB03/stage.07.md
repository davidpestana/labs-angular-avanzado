# ⚙️ Fase 3.7 – Reflexión sobre escalado de estado en apps empresariales

### 🎯 Objetivo

Evaluar críticamente el uso de NGRX (`@ngrx/store`, `@ngrx/effects`, `@ngrx/signal-store`) en aplicaciones complejas. Reflexionar sobre los patrones aplicados durante el laboratorio y tomar decisiones fundamentadas sobre cuándo aplicar cada enfoque.

---

### 🧠 Puntos de reflexión

1. **SignalStore vs Store clásico:**

   * ¿Dónde ha sido más ágil trabajar con señales locales?
   * ¿Cuándo te resulta más natural usar `select()` y `dispatch()`?

2. **Modularización:**

   * ¿La organización por `features` y `slices` ha reducido la complejidad?
   * ¿Se ha conseguido una arquitectura que escale sin duplicar lógica?

3. **Efectos y trazabilidad:**

   * ¿Es sencillo rastrear qué efecto ha lanzado qué acción?
   * ¿Te resulta fácil depurar el flujo con DevTools?

4. **Persistencia y rehidratación:**

   * ¿Persistir estado aporta valor real en este tipo de apps?
   * ¿Qué riesgos introduces cuando sincronizas el estado con `localStorage`?

5. **Testing:**

   * ¿Testear efectos es sencillo con mocks?
   * ¿Los tests de reducers ayudan a mantener calidad sin duplicar lógica?

6. **Coste/beneficio:**

   * ¿Qué overhead ha supuesto usar NGRX en vez de servicios observables o stores locales?
   * ¿Para un equipo de varios desarrolladores, crees que mejora la colaboración?

---

### ✅ Validaciones

* El alumno puede comparar patrones Signal vs Store clásico
* Se identifican ventajas de modularización y efectos bien definidos
* Se entiende cuándo aplicar uno u otro modelo de gestión de estado

---

### 💬 Cierre

¿Qué parte del stack volverías a usar en tu próximo proyecto real? ¿Qué simplificarías o evitarías? ¿En qué tipo de aplicación no te compensa usar NGRX?
