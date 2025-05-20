# 🧩 Laboratorio 5 – Angular Elements e interoperabilidad entre entornos

## 🧭 Objetivo del laboratorio

Aplicar Angular Elements para exponer componentes como Web Components reutilizables fuera del entorno Angular. El laboratorio se centra en interoperabilidad, desacoplamiento entre equipos y despliegue de funcionalidades compartidas en contextos externos (React, CMS, vanilla JS, microfrontends).

---

## 🎯 Enfoque

* Transformar componentes Angular en Custom Elements (`defineCustomElement`)
* Configurar `standalone` y `bootstrapApplication()` para construir widgets independientes
* Publicar y probar componentes en entornos no-Angular
* Abordar retos de estilo, encapsulamiento, inputs/outputs y seguridad

---

## 📅 Fases del laboratorio

### Fase 5.1 – Exportar un componente Angular como Web Component

### Fase 5.2 – Aislar el entorno con standalone y bootstrap manual

### Fase 5.3 – Inyección de datos y respuesta a eventos externos

### Fase 5.4 – Encapsulamiento de estilos y uso de Shadow DOM

### Fase 5.5 – Deploy como widget consumible (npm, CDN o HTML)

### Fase 5.6 – Integración real: React, vanilla JS o CMS externo

### Fase 5.7 – Reflexión sobre interoperabilidad, mantenimiento y testing cruzado

---

## ✅ Validaciones

* El componente funciona fuera de Angular sin runtime adicional
* Se comunica correctamente con el entorno externo vía propiedades y eventos
* Está encapsulado visualmente y es portable sin colisiones globales

---

## 💬 Reflexión

¿Hasta qué punto Angular Elements es viable para distribución de componentes? ¿Qué otras estrategias podrían ser más eficientes según el consumidor? ¿Cómo afecta esto al testing, versionado y documentación de librerías?
