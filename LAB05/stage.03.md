# ⚙️ Fase 5.3 – Inyección de datos y respuesta a eventos externos

### 🎯 Objetivo

Permitir que el Web Component reciba entradas dinámicas como atributos HTML (`@Input`) y comunique eventos hacia el exterior (`@Output`). Se busca que el componente pueda integrarse en cualquier framework o HTML plano con comunicación bidireccional.

---

### 🗂️ Scaffolding

```
libs/widget/hello-widget/
├── hello-widget.component.ts         ← define @Input y @Output
├── hello-widget.bootstrap.ts         ← inicializa el custom element
├── host.html                         ← HTML plano para pruebas
```

---

### 🪜 Pasos guiados

1. Añadir entradas y eventos al componente:

```ts
@Component({ ... })
export class HelloWidgetComponent {
  @Input() name = 'World';
  @Output() greeted = new EventEmitter<string>();

  ngOnInit() {
    this.greeted.emit(`Hola ${this.name}`);
  }
}
```

2. Verificar que los atributos HTML son aceptados:

```html
<hello-widget name="David"></hello-widget>
```

3. Escuchar eventos desde HTML:

```html
<hello-widget name="TMEIC"></hello-widget>
<script>
  const widget = document.querySelector('hello-widget');
  widget.addEventListener('greeted', e => {
    console.log('[externo]', e.detail);
  });
</script>
```

4. En el componente, emitir eventos con `CustomEvent` si se desea mayor interoperabilidad:

```ts
this.elementRef.nativeElement.dispatchEvent(
  new CustomEvent('greeted', { detail: `Hola ${this.name}` })
);
```

---

### 🎯 Retos y Tips

* **Emitir un evento cuando cambie el valor del input**

  > 💡 *Tip:* Usa `ngOnChanges` o un setter en el `@Input()` para reaccionar a cambios

* **Evitar errores si `@Input` se pasa después del `connectedCallback`**

  > 💡 *Tip:* Usa `setTimeout()` o detecta cambios con `ngOnChanges()` tras renderizado

* **Inyectar datos complejos desde JS**

  > 💡 *Tip:* Usa propiedades DOM (`element.name = 'X'`) si necesitas pasar objetos, no solo strings

---

### ✅ Validaciones

* El componente reacciona a entradas externas vía HTML o JS
* Emite eventos que pueden ser escuchados en el entorno no Angular
* El flujo de datos es bidireccional y portable

---

### 💬 Reflexión

¿Es cómodo trabajar con eventos personalizados (`CustomEvent`) desde Angular? ¿Qué problemas aparecen al sincronizar inputs dinámicos con el ciclo de vida Angular? ¿Qué framework o entorno podría beneficiarse más de esta integración?
