# FR1-gestionar-flota-operativa-cohetes. 

## 1. Problema

Gestionar flota operativa de cohetes

### Contexto

Operaciones necesita un catálogo único de cohetes para planificar lanzamientos sin hojas manuales ni fuentes contradictorias. Sin ese catálogo, la capacidad disponible y la aptitud por destino se calculan con riesgo alto de error operativo.

### Historias de usuario

- Como `operador` quiero **dar de alta y actualizar cohetes** para _mantener un inventario operativo confiable_.
- Como `planificador` quiero **consultar capacidad y rango de acción por cohete** para _asignar lanzamientos compatibles_.
- Como `supervisor` quiero **marcar estado operativo del cohete** para _evitar uso de recursos no disponibles_.

### Fuera de scope

- Mantenimiento técnico detallado por componentes del cohete.
- Telemetría en tiempo real de flota.
- Integraciones externas con fabricantes o sistemas de mantenimiento.

---

## 2.Solución

Se implementa un módulo de catálogo de cohetes con operaciones de alta, edición y consulta. El catálogo publica datos consistentes para el resto de dominios (lanzamientos y reservas), y valida el dominio de rango y estado en cada cambio.

### Modelo

#### Cohete
- id: `uuid`, identificador único interno.
- nombre: `string`, código legible del cohete.
- capacidad-maxima-pasajeros: `integer`, límite superior de plazas.
- estado-operativo: `enum(planificado-operativo|mantenimiento|fuera-de-servicio)`, disponibilidad operativa.
- rango-de-accion: `enum(tierra|luna|marte)`, destinos habilitados.
- Regla1: `capacidad-maxima-pasajeros` debe ser mayor que cero.
- Regla2: `rango-de-accion` solo acepta `tierra`, `luna` o `marte`.

### Back

API interna CRUD para cohetes (`create`, `update`, `get`, `list`) con validaciones de dominio y errores de contrato explícitos. Persistencia transaccional simple con restricciones de tipo y checks de enum para evitar estados inválidos.

### Front

Pantalla de catálogo con listado filtrable y formulario de alta/edición. Mostrar capacidad, estado y rango en columnas; aplicar validación de campos en cliente y mensajes de error claros ante reglas de dominio incumplidas.

---

## 3. Verificación

- [ ] el sistema DEBE permitir alta, modificación y consulta de cohetes con capacidad, estado y rango.
- [ ] CUANDO se intente registrar un `rango-de-accion` fuera de `tierra|luna|marte` el sistema DEBE rechazar la operación con error de validación.
- [ ] SI `capacidad-maxima-pasajeros` es menor o igual a cero ENTONCES el sistema DEBE devolver error de dominio y no persistir cambios.
- [ ] CUANDO operaciones consulte el catálogo el sistema DEBE devolver datos consistentes para planificación de lanzamientos.
