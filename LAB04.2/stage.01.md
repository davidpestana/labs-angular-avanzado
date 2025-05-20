# ⚙️ Fase 4.1 – ChangeDetection avanzada y perfiles de rendimiento

### 🎯 Objetivo

Optimizar el rendimiento de la aplicación Angular aplicando estrategias de detección de cambios eficientes (`OnPush`, signals, `trackBy`, `detach`) y evaluando el impacto de cada una con herramientas de perfilado y métricas perceptibles.

---

### 🗂️ Scaffolding

```
src/app/shared/
├── components/
│   ├── plant-list/             ← aplicar OnPush, trackBy y signals
│   └── stats-panel/            ← métricas renderizadas desde computed()
```

---

### 🪜 Pasos guiados

1. Asegúrate de que todos los componentes presentacionales usan:

```ts
changeDetection: ChangeDetectionStrategy.OnPush
```

2. Añade `trackBy` en listas largas:

```html
<li *ngFor="let item of items; trackBy: trackById">
```

```ts
trackById(index: number, item: Entity) { return item.id; }
```

3. Usa `computed()` en componentes signal:

```ts
readonly stats = computed(() => {
  const list = this.items();
  return {
    total: list.length,
    active: list.filter(i => i.status === 'active').length
  };
});
```

4. Para componentes complejos y estáticos, experimenta con `ChangeDetectorRef.detach()` + `detectChanges()` manual bajo control de eventos externos.

5. Abre Chrome DevTools → Performance → Profile y compara:

   * renderizado antes/después de aplicar OnPush
   * tiempo de respuesta al interactuar con listas y cambios de filtro

---

### 🎯 Retos y Tips

* **Identificar componentes que re-renderizan sin necesidad**

  > 💡 *Tip:* Usa DevTools + consola con `console.log()` en métodos de render para detectarlo

* \**Aplicar `trackBy` correctamente en todos los *ngFor**

  > 💡 *Tip:* No te limites a `index`; siempre que sea posible usa `item.id` o claves únicas

* **Comparar `computed()` vs `pipe(async)` en términos de eficiencia y reactividad**

  > 💡 *Tip:* Observa cómo `computed()` evita re-render si no hay cambios reales en los inputs

---

### ✅ Validaciones

* Todos los componentes visuales usan `OnPush`
* Las listas usan `trackBy`
* Los componentes con signals usan `computed()` para cálculos
* La app muestra mejoras en reactividad y menor trabajo de render

---

### 💬 Reflexión

¿Dónde ha sido más eficaz `OnPush`? ¿Qué limitaciones tiene cuando hay stores globales o múltiples fuentes de cambio? ¿Cuándo conviene `detach()` o signals en lugar de servicios clásicos?
