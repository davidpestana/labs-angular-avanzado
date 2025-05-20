### ➕ Extra opcional – Práctica avanzada con operadores RxJS y servicios derivados

#### Objetivo

Profundizar en la transformación de datos usando operadores RxJS intermedios y avanzados. Crear servicios especializados que extiendan los datos de la API con lógica de negocio reactiva, como agregados, validaciones o filtros dinámicos.

#### Propuesta

* Crear un nuevo servicio `PlantAnalyticsService` que consuma `plants$` desde `PlantDataService` y exponga:

  * `criticalPlants$`: plantas con capacidad < 100 o estado != 'active'
  * `plantsByCity$`: diccionario de ciudades → lista de plantas
  * `metrics$`: stream con `{ total: number, active: number, avgCapacity: number }`

#### Tip sugerido

```ts
export class PlantAnalyticsService {
  constructor(private data: PlantDataService) {}

  metrics$ = this.data.plants$.pipe(
    map(plants => ({
      total: plants.length,
      active: plants.filter(p => p.status === 'active').length,
      avgCapacity: Math.round(plants.reduce((sum, p) => sum + p.capacityKw, 0) / plants.length)
    }))
  );
}
```

#### Retos

* Usar `groupBy` para crear un stream de agrupación por `status`
* Crear un stream que detecte si alguna planta ha cambiado de estado entre emisiones

  > 💡 Usa `pairwise()` y `filter()` para comparar emisiones anteriores y actuales

#### Validaciones

* Los streams derivados deben ser inmutables y reactivos
* El componente debe ser capaz de suscribirse a ellos sin lógica adicional en el HTML
* El código debe poder evolucionar fácilmente a `SignalStore` o `ComponentStore`

---

### 💬 Cierre

¿Qué ha sido lo más retador? ¿Qué aplicarías de esto en tu trabajo real? ¿Cómo seguirías evolucionando esta aplicación para una siguiente iteración o equipo más grande?
