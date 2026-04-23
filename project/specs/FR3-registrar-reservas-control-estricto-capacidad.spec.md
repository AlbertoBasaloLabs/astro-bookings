# FR3-registrar-reservas-control-estricto-capacidad. 

## 1. Problema

Registrar reservas con control estricto de capacidad

### Contexto

El valor principal del MVP es evitar sobreventa. El sistema debe aceptar reservas solo con capacidad disponible real, incluso cuando llegan solicitudes concurrentes para el mismo lanzamiento.

### Historias de usuario

- Como `administrativo` quiero **crear reservas de plazas para un lanzamiento** para _confirmar ventas de forma segura_.
- Como `operaciones` quiero **bloquear reservas al alcanzar capacidad máxima** para _evitar sobreventa_.
- Como `responsable-sistema` quiero **garantizar consistencia bajo concurrencia** para _mantener integridad de negocio_.

### Fuera de scope

- Selección de asiento específico por pasajero.
- Lista de espera automática cuando no hay plazas.
- Overbooking controlado por estrategia comercial.

---

## 2.Solución

Se implementa reserva transaccional por lanzamiento con cálculo de disponibilidad dentro de la misma operación atómica. Las reservas solo se aceptan para lanzamientos en estado `planificado` y con configuración económica completa.

### Modelo

#### Reserva
- id: `uuid`, identificador de reserva.
- lanzamiento-id: `uuid`, referencia obligatoria al lanzamiento.
- plazas: `integer`, número de asientos solicitados.
- estado-reserva: `enum(pendiente|cobrada|cancelada|devuelta)`, estado comercial.
- fecha-creacion: `datetime`, trazabilidad temporal.
- Regla1: `plazas` debe ser mayor que cero.
- Regla2: la suma de plazas reservadas y cobradas nunca excede la capacidad del cohete del lanzamiento.

### Back

Endpoint de creación de reserva con lock transaccional por `lanzamiento-id` (o estrategia equivalente de control de concurrencia). Validar estado del lanzamiento y capacidad remanente en la misma transacción antes de persistir.

### Front

Formulario de reserva con indicador de plazas disponibles en tiempo casi real. Al confirmar, mostrar resultado definitivo de aceptación/rechazo y motivo de negocio cuando no haya capacidad.

---

## 3. Verificación

- [ ] el sistema DEBE registrar reservas solo si existen plazas disponibles.
- [ ] CUANDO dos solicitudes concurrentes compitan por las últimas plazas el sistema DEBE aceptar solo las que respeten la capacidad total.
- [ ] SI una reserva supera la capacidad disponible ENTONCES el sistema DEBE rechazar la operación sin efectos parciales.
- [ ] CUANDO un lanzamiento esté `suspendido` o `completado` el sistema DEBE bloquear nuevas reservas.
