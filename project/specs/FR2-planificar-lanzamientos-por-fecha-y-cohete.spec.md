# FR2-planificar-lanzamientos-por-fecha-y-cohete. Planificar lanzamientos por fecha y cohete

## 1. Problema

### Contexto

La planificación de lanzamientos requiere asignar un único cohete por salida y evitar conflictos de uso del mismo recurso. Además, cada lanzamiento debe quedar preparado comercialmente con precio y umbral mínimo de ocupación antes de abrir reservas.

### Historias de usuario

- Como `planificador` quiero **crear lanzamientos con fecha, cohete y destino** para _publicar un calendario operativo_.
- Como `operador` quiero **evitar solapamientos incompatibles del mismo cohete** para _prevenir conflictos de ejecución_.
- Como `responsable-comercial` quiero **definir precio y umbral mínimo de ocupación** para _evaluar continuidad económica del lanzamiento_.

### Fuera de scope

- Optimización automática de calendario basada en IA.
- Planificación multi-cohete en un mismo lanzamiento.
- Reprogramación automática ante incidencias climáticas.

---

## 2.Solución

Se crea un módulo de calendario de lanzamientos que valida asignación única de cohete, no solapamiento incompatible y configuración económica obligatoria. El lanzamiento nace en estado `planificado` y queda listo para reservas si cumple precondiciones.

### Modelo

#### Lanzamiento
- id: `uuid`, identificador único.
- cohete-id: `uuid`, referencia obligatoria a `cohete`.
- fecha-hora-salida: `datetime`, instante programado.
- precio-por-asiento: `decimal`, importe unitario.
- umbral-minimo-ocupacion: `integer`, porcentaje de 0 a 100.
- estado-de-lanzamiento: `enum(planificado|suspendido|completado)`, ciclo de vida base.
- Regla1: un cohete no puede tener lanzamientos incompatibles en la misma ventana temporal.
- Regla2: no se habilitan reservas sin `precio-por-asiento` y `umbral-minimo-ocupacion` válidos.
- Regla3: un cohete no puede ser lanzado en los 10 días posteriores a su último lanzamiento.

### Back

Endpoints para crear y consultar lanzamientos. En creación, validar existencia de cohete, compatibilidad de destino con rango del cohete y ausencia de solapamiento para el recurso. Persistir restricciones de unicidad temporal por cohete según política operativa.

### Front

Vista calendario/lista de lanzamientos con formulario de alta. Mostrar advertencias de conflicto antes de guardar y feedback inmediato sobre reglas económicas faltantes.

---

## 3. Verificación

- [ ] el sistema DEBE crear lanzamientos asociados a exactamente un cohete.
- [ ] CUANDO se intente crear un lanzamiento solapado para el mismo cohete el sistema DEBE rechazar la operación.
- [ ] SI falta `precio-por-asiento` o `umbral-minimo-ocupacion` ENTONCES el sistema DEBE impedir habilitar reservas para ese lanzamiento.
- [ ] CUANDO se consulte el calendario el sistema DEBE mostrar fecha, cohete, precio y umbral de cada lanzamiento.
- [ ] CUANDO planifique un lanzamiento el sistema DEBE verificar que el cohete no haya sido lanzado en los últimos 10 días.