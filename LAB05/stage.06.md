# ⚙️ Fase 5.6 – Integración real: React, vanilla JS o CMS externo

### 🎯 Objetivo

Demostrar que el componente generado puede integrarse en distintos entornos reales, desde aplicaciones en otros frameworks (React, Vue) hasta páginas estáticas, CMS o editores visuales. Se probará la interoperabilidad y respuesta bidireccional del widget en sistemas no Angular.

---

### 🗂️ Scaffolding

```
integrations/
├── react-app/                  ← integración con React
├── vanilla-html/              ← integración directa con JS plano
├── cms-snippet.html           ← código para insertar en WordPress, Webflow, etc.
```

---

### 🪜 Pasos guiados

1. **Integración con React:**

```tsx
useEffect(() => {
  const widget = document.querySelector('hello-widget');
  widget.setAttribute('name', 'React User');
  widget.addEventListener('greeted', e => console.log('Desde React:', e.detail));
}, []);

return <hello-widget />;
```

2. **Integración en HTML estático o JS vanilla:**

```html
<hello-widget id="demo"></hello-widget>
<script>
  document.getElementById('demo').setAttribute('name', 'Vanilla');
</script>
```

3. **Integración en CMS externo (ej. WordPress):**

```html
<!-- en el editor HTML personalizado -->
<script src="https://cdn.example.com/hello-widget.js"></script>
<hello-widget name="CMS User"></hello-widget>
```

4. Verifica interoperabilidad:

* El componente carga sin errores
* Responde correctamente a propiedades y eventos
* Conserva su aislamiento visual y funcional

---

### 🎯 Retos y Tips

* **Pasar datos desde React sin perder tipado**

  > 💡 *Tip:* Usa `ref` o `useRef()` para obtener el DOM y manipularlo desde `useEffect()`

* **Insertar el widget en una web con jQuery o editor visual**

  > 💡 *Tip:* Verifica compatibilidad y evita conflictos con globales o duplicados

* **Medir comportamiento con múltiples instancias en el mismo entorno**

  > 💡 *Tip:* Escucha eventos por `id` o con lógica de identificación si es necesario

---

### ✅ Validaciones

* El widget se carga sin Angular en apps de React o CMS
* Permite configuración y escucha de eventos desde fuera
* Conserva estilos y funcionalidad sin depender del entorno externo

---

### 💬 Reflexión

¿Es esta integración tan simple como parece? ¿Qué complicaciones aparecen al versionar widgets para múltiples frameworks? ¿Es más conveniente una estrategia de microfrontend o de librería universal?
