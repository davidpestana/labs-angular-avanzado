# ⚙️ Fase 3.5 – Persistencia selectiva y sincronización con localStorage

### 🎯 Objetivo

Persistir partes específicas del estado global de forma automática usando `meta-reducers`, para mantener la experiencia del usuario entre recargas. Aprender a hidratar el store desde `localStorage` y a excluir propiedades volátiles como `loading` o `error`.

---

### 🗂️ Scaffolding

```
src/app/core/state/
├── meta-reducers/
│   ├── local-storage.metareducer.ts

src/app/app.config.ts
├── provideStore(..., { metaReducers })
```

---

### 🪜 Pasos guiados

1. Crear el meta-reducer:

```ts
export function localStorageSyncMetaReducer(reducer: ActionReducer<any>): ActionReducer<any> {
  return function (state, action) {
    const nextState = reducer(state, action);
    const partial = {
      plants: {
        ...nextState.plants,
        loading: false,
        error: null
      }
    };
    localStorage.setItem('appState', JSON.stringify(partial));
    return nextState;
  };
}
```

2. Hidratar el estado al iniciar:

```ts
export const getInitialState = () => {
  const saved = localStorage.getItem('appState');
  return saved ? JSON.parse(saved) : {};
};
```

3. Registrar el meta-reducer:

```ts
provideStore(reducers, {
  metaReducers: [localStorageSyncMetaReducer],
  initialState: getInitialState()
})
```

---

### 🎯 Retos

* Excluir del guardado propiedades como `selectedId`, `loading`, `error`

  > 💡 *Tip:* Puedes usar destructuring y `delete` dentro del `meta-reducer` antes de guardar el estado.

* Crear una función `clearPersistedState()` para debugging manual

  > 💡 *Tip:* Añade `localStorage.removeItem('appState')` en un `util.ts` o en una opción del menú de desarrollo.

* Persistir también filtros del usuario dentro de `plants.filter`

  > 💡 *Tip:* Si `filter` es parte del slice, asegúrate de incluirlo en `partial.plants` y evitar sobreescribirlo desde el backend tras la carga.

---

### ✅ Validaciones

* Al recargar, se restaura automáticamente el estado persistido
* Solo se guardan propiedades deseadas (no volátiles)
* La UX mantiene contexto sin depender del backend

---

### 💬 Reflexión

¿Hasta qué punto conviene persistir el estado del store? ¿Qué problemas podrían surgir con estados stale o inconsistentes con el backend?
