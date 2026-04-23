# AstroBookings — Requerimientos

## 1. Requerimientos Funcionales

### FR1 — Gestionar flota operativa de cohetes
El sistema debe permitir mantener un catálogo interno de cohetes con su capacidad máxima de pasajeros, su estado operativo y su rango de acción (`tierra`, `luna` o `marte`), de forma que operaciones disponga siempre de una base confiable para planificar lanzamientos y asignar reservas sin cálculos manuales.

### FR2 — Planificar lanzamientos por fecha y cohete
El sistema debe permitir crear y consultar lanzamientos programados, asignando un único cohete por lanzamiento y evitando solapamientos operativos incompatibles para el mismo recurso. En cada lanzamiento se debe definir precio por asiento y umbral mínimo de ocupación para evaluar su continuidad operativa.

### FR3 — Registrar reservas con control estricto de capacidad
El sistema debe permitir registrar reservas para un lanzamiento únicamente mientras existan plazas disponibles. Nunca debe aceptar reservas que superen la capacidad del cohete asignado al lanzamiento, incluyendo operaciones concurrentes.

### FR4 — Gestionar ciclo de vida de lanzamientos
El sistema debe soportar la evolución de estado de cada lanzamiento usando un flujo simple para MVP (`planificado`, `suspendido`, `completado`). Cuando un lanzamiento se suspenda, se debe registrar la causa (`económica`, `técnica` o `climática`) de forma explícita.

### FR5 — Gestionar cobros y devoluciones de reservas
El sistema debe permitir registrar el cobro de una reserva y procesar su devolución cuando aplique por suspensión del lanzamiento u otros motivos operativos, utilizando una pasarela de pago ficticia orientada a formación.

---

## 2. Requerimientos Técnicos

### TR1 — Seguridad
La versión MVP no incluye autenticación ni autorización. Se asume entorno controlado de taller, y cualquier requisito de seguridad queda diferido a fases posteriores.

### TR2 — Rendimiento
Las operaciones habituales de consulta y registro (flota, calendario y reservas) deben responder con latencia percibida como interactiva en entorno de taller. Se prioriza consistencia de reglas de negocio sobre optimización extrema.

### TR3 — Confiabilidad
La creación de reservas debe ejecutarse de forma atómica para evitar sobreventa. Los errores de dominio deben devolverse de forma explícita y consistente. El sistema debe preservar integridad de estados incluso ante fallos intermedios.

### TR4 — Observabilidad
Debe existir trazabilidad mínima de eventos de negocio clave: alta/modificación de lanzamiento, cambio de estado y creación/cancelación de reserva. Los registros deben permitir reconstruir decisiones operativas durante ejercicios de formación.

### TR5 — Integraciones
No se contemplan integraciones reales con pagos, ERP ni proveedores externos. Para pagos se usará una pasarela ficticia simulada con contratos estables, manteniendo fronteras claras para una futura integración real.

---

## 3. Dominio del problema

### 3.1. Entidades y lenguaje ubicuo

- `cohete`: recurso físico con capacidad máxima y estado operativo.
- `rango-de-acción`: destino operativo permitido para un cohete (`tierra`, `luna`, `marte`).
- `lanzamiento`: salida planificada que usa un único cohete en una fecha/hora determinada.
- `precio-por-asiento`: importe unitario configurado para un lanzamiento.
- `umbral-mínimo-ocupación`: porcentaje mínimo de plazas vendidas requerido para evitar suspensión económica.
- `reserva`: asignación de una o más plazas a un lanzamiento concreto.
- `capacidad-disponible`: plazas restantes en un lanzamiento en un momento dado.
- `estado-de-lanzamiento`: fase del ciclo de vida operativo de un lanzamiento.
- `causa-de-suspension`: causa registrada cuando un lanzamiento pasa a `suspendido` (`económica`, `técnica`, `climática`).
- `cobro`: operación económica de confirmación de pago de una reserva.
- `devolución`: operación económica de reembolso asociada a una reserva cobrada.

### 3.2. Reglas de Negocio

- Un `lanzamiento` debe estar asociado a exactamente un `cohete`.
- El `rango-de-acción` de un `cohete` solo puede ser `tierra`, `luna` o `marte`.
- La suma de plazas reservadas en un `lanzamiento` nunca puede exceder la capacidad del `cohete` asignado.
- Cada `lanzamiento` debe definir `precio-por-asiento` y `umbral-mínimo-ocupación` antes de aceptar reservas.
- Si un mes antes del lanzamiento la ocupación es inferior al `umbral-mínimo-ocupación`, pasará a `suspendido` por causa económica.
- Motivos climáticos o técnicos también pueden causar la suspensión de un lanzamiento.
- No se permiten nuevas `reservas` cuando el `estado-de-lanzamiento` sea `suspendido` o `completado`.
- Una `devolución` solo puede ejecutarse sobre una `reserva` previamente `cobrada`.
- Cada cambio de `estado-de-lanzamiento` debe registrar fecha y actor; en `suspendido` debe registrar también `causa-de-suspension`.
- El ciclo de vida permitido para `estado-de-lanzamiento` es:

| Estado actual | Puede pasar a |
| --- | --- |
| planificado | suspendido, completado |
| suspendido | (sin transición) |
| completado | (sin transición) |
