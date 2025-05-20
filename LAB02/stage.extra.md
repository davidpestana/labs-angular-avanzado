### 🎯 Reto extra: Persistencia de estado en localStorage

Implementa persistencia del estado de la store en `localStorage` para mantener la sesión del usuario entre recargas.

**💡 Tips:**

* Lee el estado inicial desde `localStorage` al construir la `ComponentStore`:

  ```ts
  const saved = localStorage.getItem('plantState');
  const initialState = saved ? JSON.parse(saved) : DEFAULT_STATE;
  super(initialState);
  ```
* Suscríbete a `this.state$` y guarda cambios:

  ```ts
  this.state$.pipe(
    debounceTime(300),
    distinctUntilChanged()
  ).subscribe(state => {
    localStorage.setItem('plantState', JSON.stringify(state));
  });
  ```
* Considera excluir propiedades como `loading` o `error` de la persistencia.

**Objetivo del reto:** Consolidar la arquitectura reactiva con un mecanismo de persistencia básico y reflexionar sobre las implicaciones de mantener estado cliente-side entre sesiones.

¿Este patrón te resulta cómodo para proyectos futuros? ¿Qué elementos adaptarías si el equipo fuera más grande o las vistas más complejas? ¿Qué parte del flujo ha resultado más clara o más repetitiva?
