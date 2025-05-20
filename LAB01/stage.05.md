# ⚙️ Fase 1.5 – Composición de streams en la vista

### 🎯 Objetivo

Aplicar operadores de RxJS para transformar, filtrar y enriquecer flujos de datos en la capa de presentación. Generar streams derivados para estadísticas, indicadores visuales y mensajes dinámicos. Fortalecer la separación entre lógica y presentación.

---

### 🗂️ Scaffolding

```
src/app/pages/dashboard/
├── dashboard.page.ts          ← streams derivados y lógica extendida
├── dashboard.page.html        ← renderizado reactivo con pipes y condiciones
```

---

### 🪜 Pasos guiados

1. Derivar número total de plantas:

```ts
plantCount$ = this.plants$.pipe(
  map(plants => plants.length)
);
```

2. Filtrar solo las plantas activas:

```ts
activePlants$ = this.plants$.pipe(
  map(plants => plants.filter(p => p.status === 'active'))
);
```

3. Calcular capacidad total instalada:

```ts
totalCapacity$ = this.plants$.pipe(
  map(plants => plants.reduce((sum, p) => sum + p.capacityKw, 0))
);
```

4. Añadir visualización en plantilla:

```html
<p>Total de plantas: {{ plantCount$ | async }}</p>
<p>Capacidad instalada: {{ totalCapacity$ | async }} kW</p>

<h3>Plantas activas</h3>
<ul>
  <li *ngFor="let plant of activePlants$ | async">
    {{ plant.name }} – {{ plant.capacityKw }} kW
  </li>
</ul>
```

---

### 🎯 Retos

* Añadir un botón para mostrar u ocultar las plantas inactivas

  > 💡 *Tip:* Usa un `BehaviorSubject<boolean>` para controlar el estado y combínalo con `combineLatest`

* Mostrar un mensaje dinámico según el total de plantas

  > 💡 *Tip:* Usa `.map(count => count > 50 ? 'Alta escala' : 'Pequeña red')`

* Generar un stream que agrupe plantas por `location`

  > 💡 *Tip:* Usa `reduce()` para generar un objeto `{ [city]: Plant[] }`

---

### ✅ Validaciones

* Todos los datos derivados se generan desde el observable original `plants$`
* No hay lógica de transformación dentro del HTML
* El rendimiento visual mejora gracias a `async` y operadores reactivos

---

### 💬 Reflexión

¿Dónde situar la frontera entre lo que se transforma en el componente y lo que debe delegarse al servicio? ¿Qué criterio seguir para no duplicar lógica?
