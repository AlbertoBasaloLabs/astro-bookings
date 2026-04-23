# TR5-integraciones-ficticias-y-fronteras-claras. Integraciones ficticias y fronteras claras

## 1. Problema

### Contexto

El MVP no debe depender de sistemas externos reales (pagos, ERP, proveedores), pero sí preparar límites técnicos para reemplazar simulaciones en el futuro. Sin fronteras claras, migrar de pasarela ficticia a integración real será costoso y riesgoso.

### Historias de usuario

- Como `equipo-tecnico` quiero **usar una pasarela ficticia estable** para _desarrollar y probar sin dependencias externas_.
- Como `arquitecto` quiero **definir contratos de integración desacoplados** para _sustituir adaptadores sin romper dominio_.
- Como `formador` quiero **simular escenarios de pago de forma reproducible** para _ejercicios consistentes_.

### Fuera de scope

- Conexión real con PSP, ERP o APIs de terceros.
- Gestión de credenciales productivas.
- Monitoreo de SLA de integraciones externas reales.

---

## 2.Solución

Se implementa patrón de puerto/adaptador para pagos con contrato estable en dominio y adaptador ficticio para MVP. Cualquier integración real futura se limita a sustituir el adaptador sin alterar casos de uso principales.

### Modelo

#### Contrato-pasarela-pago
- operacion: `enum(cobrar|devolver)`, acción solicitada.
- referencia-interna: `string`, id de reserva/movimiento.
- importe: `decimal`, monto a procesar.
- resultado: `enum(confirmado|rechazado)`, outcome de la pasarela.
- referencia-externa: `string`, identificador del proveedor/adaptador.
- Regla1: el dominio no conoce detalles internos del proveedor concreto.
- Regla2: el contrato de respuesta debe ser estable para todos los adaptadores.

### Back

Definir interfaz de pasarela de pagos y adaptador ficticio por defecto. Inyectar adaptador mediante configuración para permitir swap futuro a integración real sin cambiar lógica de negocio.

### Front

La UI consume únicamente estados funcionales de cobro/devolución y no detalles del proveedor. Mensajes orientados a negocio, no a infraestructura.

---

## 3. Verificación

- [ ] el sistema DEBE operar pagos en MVP mediante pasarela ficticia simulada.
- [ ] CUANDO se ejecute cobro o devolución el sistema DEBE usar un contrato estable independiente del proveedor.
- [ ] SI se reemplaza el adaptador ficticio por uno real ENTONCES el sistema DEBE mantener intactos los casos de uso de dominio.
- [ ] CUANDO se revisen fronteras de integración el sistema DEBE mostrar separación clara entre dominio y adaptadores externos.
