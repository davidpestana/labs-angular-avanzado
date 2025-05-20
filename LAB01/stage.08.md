# ⚙️ Fase 1.8 – Reflexión final sobre arquitectura reactiva

### 🎯 Objetivo

Cerrar el laboratorio consolidando los aprendizajes sobre programación reactiva en Angular. Analizar cómo la arquitectura basada en observables, tipado fuerte y separación de responsabilidades influye en la escalabilidad y mantenibilidad de proyectos reales.

---

### 🧠 Puntos de debate para el grupo

1. **Declaratividad vs. Imperatividad:**

   * ¿Cómo ha cambiado tu forma de pensar la lógica de presentación tras usar RxJS?
   * ¿Qué casos siguen siendo más fáciles de escribir de forma imperativa?

2. **Gestión de estados:**

   * ¿Fue fácil mantener la claridad con `BehaviorSubject`, `combineLatest` y múltiples flujos?
   * ¿Se justificaba introducir una store más robusta como `ComponentStore` o `SignalStore`?

3. **Separación de responsabilidades:**

   * ¿Los servicios encapsulan bien los datos y la lógica?
   * ¿Los componentes quedaron limpios y centrados en visualización?

4. **Tipado automático vs. manual:**

   * ¿Qué mejoras trajo el uso de OpenAPI Generator?
   * ¿Hay alguna desventaja en depender de la estructura generada automáticamente?

5. **Valor del backend simulado:**

   * ¿El backend JSON ha sido suficiente para detectar problemas de asincronía y validación?
   * ¿Cómo habría cambiado tu enfoque si trabajaras sobre una API real?

6. **Escalabilidad del patrón:**

   * ¿Esta arquitectura podría mantenerse en un equipo grande?
   * ¿Qué piezas modularizarías más aún si el proyecto creciera?

---

### ✅ Validaciones

* Todos los alumnos pueden identificar qué partes del flujo usan RxJS
* Se reconoce el valor añadido de `async`, `shareReplay`, `combineLatest`, `catchError`, etc.
* Se identifican al menos 3 decisiones técnicas que facilitarían la evolución del proyecto

---

### 💬 Cierre

¿Qué ha sido lo más retador? ¿Qué aplicarías de esto en tu trabajo real? ¿Cómo seguirías evolucionando esta aplicación para una siguiente iteración o equipo más grande?
