# TR4-observabilidad. 

## 1. Problema

Observabilidad

### Contexto

El taller requiere reconstruir decisiones operativas: qué lanzamiento se creó o cambió, cuándo se modificó su estado y qué reservas se crearon o cancelaron. Sin trazabilidad mínima, no se puede auditar ni explicar el comportamiento del sistema.

### Historias de usuario

- Como `supervisor` quiero **consultar trazas de eventos clave** para _entender decisiones operativas_.
- Como `formador` quiero **reconstruir una secuencia de acciones** para _analizar casos en clase_.
- Como `soporte` quiero **correlacionar errores con eventos de negocio** para _acelerar diagnóstico_.

### Fuera de scope

- Plataforma completa de observabilidad distribuida.
- Retención histórica de largo plazo con políticas legales.
- Dashboards avanzados con métricas de negocio en tiempo real.

---

## 2.Solución

Se define logging estructurado mínimo para eventos de negocio clave: alta/modificación de lanzamiento, cambio de estado y creación/cancelación de reserva. Cada evento incluye identificadores de entidad, actor y timestamp para reconstrucción cronológica.

### Modelo

#### Evento-negocio
- id-evento: `uuid`, identificador único del evento.
- tipo-evento: `enum(lanzamiento-creado|lanzamiento-modificado|lanzamiento-estado-cambiado|reserva-creada|reserva-cancelada)`, categoría del evento.
- entidad-id: `uuid`, agregado afectado.
- actor: `string`, origen del cambio.
- fecha-hora: `datetime`, momento de ocurrencia.
- payload-resumen: `json`, campos relevantes del cambio.
- Regla1: todo cambio de estado de lanzamiento debe emitir evento.
- Regla2: la cancelación/creación de reserva debe quedar trazada de forma inequívoca.

### Back

Emitir eventos en capa de aplicación tras confirmar transacciones de negocio. Persistir o registrar en salida estructurada (json log) con esquema estable para análisis posterior.

### Front

No requiere UI compleja en MVP; opcionalmente exponer vista simple de historial por lanzamiento/reserva para soporte y formación.

---

## 3. Verificación

- [ ] el sistema DEBE registrar eventos de alta/modificación de lanzamiento, cambio de estado y creación/cancelación de reserva.
- [ ] CUANDO un lanzamiento cambie de estado el sistema DEBE incluir actor y timestamp en el registro.
- [ ] SI falla la emisión de trazabilidad ENTONCES el sistema DEBE señalizar incidencia operativa sin ocultar el error.
- [ ] CUANDO se audite un caso de taller el sistema DEBE permitir reconstruir la secuencia de decisiones operativas.
