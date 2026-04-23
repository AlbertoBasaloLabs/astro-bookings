# FR5-gestionar-cobros-devoluciones-reservas. 

## 1. Problema

Gestionar cobros y devoluciones de reservas

### Contexto

La operación necesita cerrar el ciclo comercial de la reserva: cobrar cuando procede y devolver cuando el lanzamiento se suspende u ocurre incidencia operativa. En MVP, esto debe hacerse con pasarela ficticia para mantener foco didáctico.

### Historias de usuario

- Como `administrativo` quiero **registrar el cobro de una reserva** para _confirmar ingresos operativos_.
- Como `operador` quiero **devolver una reserva cobrada cuando aplica** para _corregir impacto al cliente_.
- Como `supervisor` quiero **trazar cada operación económica** para _auditar decisiones en talleres_.

### Fuera de scope

- Integración real con PSP bancaria.
- Conciliación contable y facturación fiscal completa.
- Gestión avanzada de fraude.

---

## 2.Solución

Se añade subdominio de pagos con operaciones `cobro` y `devolución` sobre reserva. Ambas consumen una pasarela simulada con contrato estable. La devolución exige precondición de reserva previamente cobrada.

### Modelo

#### Movimiento-pago
- id: `uuid`, identificador de movimiento.
- reserva-id: `uuid`, reserva asociada.
- tipo: `enum(cobro|devolución)`, naturaleza del movimiento.
- importe: `decimal`, monto del movimiento.
- estado: `enum(pendiente|confirmado|rechazado)`, resultado de pasarela ficticia.
- referencia-pasarela: `string`, id devuelto por adaptador de pago.
- Regla1: una `devolución` solo se permite sobre reservas `cobradas`.
- Regla2: el importe de devolución no puede superar el total cobrado en la reserva.

### Back

Endpoints para ejecutar cobro y devolución que orquestan el adaptador ficticio de pagos. Persistir resultado y actualizar `estado-reserva` según outcome. Devolver errores de negocio claros (no cobrada, ya devuelta, importe inválido).

### Front

En detalle de reserva, mostrar acciones de cobrar/devolver según estado actual. Mostrar histórico de movimientos económicos y estado devuelto por la pasarela simulada.

---

## 3. Verificación

- [ ] el sistema DEBE permitir cobrar una reserva elegible y registrar el resultado del movimiento.
- [ ] CUANDO un lanzamiento se suspenda y la reserva esté cobrada el sistema DEBE permitir procesar su devolución.
- [ ] SI la reserva no está cobrada ENTONCES el sistema DEBE rechazar la devolución.
- [ ] CUANDO se complete un cobro o devolución el sistema DEBE actualizar estado de reserva y registrar trazabilidad del movimiento.
