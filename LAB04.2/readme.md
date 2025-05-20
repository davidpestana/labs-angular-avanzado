# 🛡️ Laboratorio 4 – Performance, Seguridad y Modularización Extrema

## 🧭 Objetivo del laboratorio

A partir del sistema ya escalado con estado compartido (`@ngrx/store` + `signal-store`), este laboratorio se centra en preparar la aplicación para producción. Se aplican prácticas avanzadas de rendimiento, control de acceso, lazy-loading completo, y encapsulación de microdominios en bibliotecas o módulos de librería.

---

## 🎯 Enfoque

* Optimización de renderizado con `ChangeDetectionStrategy.OnPush`, `trackBy`, signals y `detach()`
* Seguridad de rutas, tokens y visibilidad condicional basada en roles
* Carga progresiva de módulos y recursos sensibles (alerts, usuarios)
* División de dominio en bibliotecas independientes (Nx o manual)

---

## 📅 Fases del laboratorio

### Fase 4.1 – ChangeDetection avanzada y perfiles de rendimiento

### Fase 4.2 – Protección de rutas y visibilidad condicional por permisos

### Fase 4.3 – Lazy loading profundo por dominio + preload selectivo

### Fase 4.4 – Migración de features a librerías desacopladas (Nx/lib)

### Fase 4.5 – Auditoría de seguridad básica y política de acceso de componentes

### Fase 4.6 – Bundles, análisis de dependencias y presupuestos de compilación

### Fase 4.7 – Reflexión final sobre arquitectura de producción

---

## 🧭 Método de trabajo para este laboratorio

Este laboratorio no parte de cero: se basa en la evolución del proyecto anterior, que ya cuenta con estado global, SignalStore, y slices funcionales. Ahora daremos un paso más profesionalizando su estructura y preparando su modularización para escenarios reales.

En lugar de reiniciar con un proyecto vacío en Nx, abordaremos el reto de **refactorizar progresivamente el sistema actual** hacia una organización más escalable. Esta transición permitirá valorar la modularización como estrategia de mantenimiento, sin perder funcionalidad ni contexto.

✔️ Aplicaremos Nx en un dominio ya conocido, reutilizando lógica existente y reorganizándola con intención arquitectónica.
✔️ Practicaremos decisiones reales de separación por responsabilidad: qué entra en cada lib, qué se mantiene como app.
✔️ Al finalizar, tendrás un proyecto modular, testable, y preparado para evolucionar por equipos o contextos distintos.

Como extensión opcional, podrás aplicar lo aprendido desde cero con un Nx limpio, pero el enfoque principal de este laboratorio es consolidar habilidades a través de una migración fundamentada.

---

## ✅ Validaciones

* La app responde rápido y no renderiza innecesariamente
* Cada dominio puede ser extraído o compartido sin dependencias circulares
* Las rutas, componentes y stores están protegidas por permisos
* Se ha medido y optimizado el impacto de cada mejora

---

## 💬 Reflexión

¿Esta arquitectura es sostenible en producción? ¿Qué parte sería un cuello de botella en tu equipo? ¿Qué sacrificarías para ganar simplicidad sin comprometer calidad?
