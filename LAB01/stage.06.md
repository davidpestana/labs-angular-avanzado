# ⚙️ Fase 1.6 – Estados intermedios y UX reactiva

### 🎯 Objetivo

Gestionar de forma reactiva los estados de carga, error y contenido vacío. Aprender a combinar streams derivados con `tap`, `catchError` y `startWith`, y a comunicar al usuario lo que está ocurriendo en cada momento.

---

### 🗂️ Scaffolding

```
src/app/pages/dashboard/
├── dashboard.page.ts          ← gestión de estados
├── dashboard.page.html        ← visualización condicional avanzada
```

---

### 🪜 Pasos guiados

1. Añadir variables de estado como `loading$`, `error$`, `hasData$`:

```ts
loading$ = new BehaviorSubject<boolean>(true);
error$ = new BehaviorSubject<string | null>(null);
hasData$ = this.plants$.pipe(map(plants => plants.length > 0));
```

2. Modificar `getPlants$` para emitir estados:

```ts
getPlants$(): Observable<Plant[]> {
  this.loading$.next(true);
  this.error$.next(null);
  return this.http.get<Plant[]>(this.apiUrl).pipe(
    delay(500),
    tap(() => this.loading$.next(false)),
    catchError(err => {
      this.error$.next('Error al cargar las plantas');
      this.loading$.next(false);
      return of([]);
    }),
    shareReplay(1)
  );
}
```

3. Mostrar estos estados en plantilla:

```html
<ng-container *ngIf="loading$ | async">Cargando plantas...</ng-container>
<ng-container *ngIf="error$ | async as error">Error: {{ error }}</ng-container>
<ng-container *ngIf="(hasData$ | async) === false">No hay datos disponibles</ng-container>
```

4. Combinar streams para simplificar lógica de interfaz:

```ts
vm$ = combineLatest({
  plants: this.plants$,
  loading: this.loading$,
  error: this.error$,
  hasData: this.hasData$
});
```

---

### 🎯 Retos

* Mostrar un botón "Reintentar" si hay error

  > 💡 *Tip:* Usa un método `reload()` que vuelva a llamar a `getPlants$()` y reinicie estados

* Mostrar mensaje "Sin resultados" si hay conexión pero el array está vacío

  > 💡 *Tip:* Usa `filter(plants => !loading && !error)` para mostrar contenido condicional

* Emitir el estado actual como un string combinando los tres (`'loading' | 'error' | 'ok' | 'empty'`)

  > 💡 *Tip:* Usa `combineLatest` y un `map()` con condiciones

---

### ✅ Validaciones

* La vista representa correctamente los 4 estados posibles
* El observable base no cambia, solo se derivan comportamientos
* El usuario puede reintentar una carga fallida

---

### 💬 Reflexión

¿Este modelo de estados reactivos escala bien cuando hay múltiples fuentes de datos? ¿Qué diferencia hay entre mantener estados con subjects vs. stores dedicados?
