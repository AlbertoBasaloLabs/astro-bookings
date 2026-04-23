# FR4-gestionar-ciclo-de-vida-lanzamientos. Gestionar ciclo de vida de lanzamientos

## 1. Problema

### Contexto

Operaciones necesita representar el estado real de cada lanzamiento con un flujo corto, auditable y sin ambigüedades. La suspensión exige causa explícita para análisis operativo y ejercicios formativos.

### Historias de usuario

- Como `operador` quiero **cambiar estado de lanzamiento según su situación operativa** para _mantener el calendario actualizado_.
- Como `supervisor` quiero **registrar causa al suspender un lanzamiento** para _documentar decisiones y acciones_.
- Como `administrativo` quiero **bloquear operaciones no válidas por estado** para _evitar inconsistencias en reservas y cobros_.

### Fuera de scope

- Flujos extendidos de estados (reprogramado, en curso, cerrado contable).
- Gestión de múltiples causas por una misma suspensión.
- Motor de workflow configurable por usuario.

---

## 2.Solución

Se define máquina de estados cerrada para `lanzamiento`: `planificado -> suspendido|completado`, con estados terminales para `suspendido` y `completado`. Toda transición persiste actor, fecha y, en suspensión, causa obligatoria.

### Modelo

#### Estado-lanzamiento
- lanzamiento-id: `uuid`, referencia al agregado.
- estado-actual: `enum(planificado|suspendido|completado)`, valor vigente.
- causa-de-suspension: `enum(económica|técnica|climática|null)`, obligatoria solo en `suspendido`.
- fecha-cambio-estado: `datetime`, marca temporal del cambio.
- actor-cambio-estado: `string`, identificador del actor de operación.
- Regla1: solo se permiten transiciones declaradas en el PRD.
- Regla2: al pasar a `suspendido` se exige `causa-de-suspension`.

### Back

Endpoint de transición de estado con validación del estado origen y destino. Registrar evento de negocio por cada cambio, incluyendo metadatos de actor y timestamp. Exponer error de dominio si transición no permitida.

### Front

Detalle de lanzamiento con estado visible y acción de cambio condicionada por estado actual. Si destino es `suspendido`, exigir selección de causa antes de confirmar.

---

## 3. Verificación

- [ ] el sistema DEBE permitir solo el flujo `planificado -> suspendido|completado`.
- [ ] CUANDO un lanzamiento pase a `suspendido` el sistema DEBE requerir causa `económica|técnica|climática`.
- [ ] SI se solicita una transición no permitida ENTONCES el sistema DEBE rechazarla con error de dominio.
- [ ] CUANDO se confirme un cambio de estado el sistema DEBE registrar fecha y actor del cambio.
