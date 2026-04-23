# AstroBookings

Plataforma de gestión interna para una empresa ficticia de turismo espacial que necesita planificar lanzamientos y operar reservas de forma ordenada en una primera versión MVP.

## 1. Idea

### 1.1 Problema o necesidad
AstroBookings necesita coordinar su operación diaria sin errores de capacidad ni conflictos de calendario entre cohetes y lanzamientos. Además, debe decidir la viabilidad económica de cada salida según ocupación mínima esperada y gestionar cobros/devoluciones de reservas. Sin una herramienta unificada, aumentan la sobreventa, decisiones lentas ante incidencias y pérdidas por lanzamientos no rentables.

### 1.2 Misión u objetivo
Diseñar y validar, en un contexto formativo, un sistema de referencia MVP que permita gestionar flota, lanzamientos, reservas y cobros con reglas claras de negocio, reduciendo a cero las sobre-reservas y permitiendo suspender o cancelar lanzamientos con trazabilidad económica y técnica.

### 1.3 Público objetivo
- Personal de operaciones que planifica lanzamientos.
- Personal administrativo/comercial que registra y revisa reservas.
- Responsables internos que supervisan estado de vuelos y ocupación.
- Formadores y estudiantes que usarán el proyecto como caso práctico.

---

## 2. Alcance

### 2.1 Qué se incluye
- Gestión de flota de cohetes (alta, consulta, capacidad operativa y rango de acción: Tierra, Luna o Marte).
- Planificación de lanzamientos con estados operativos, precio por asiento y umbral mínimo de ocupación.
- Registro y control de reservas sin superar la capacidad del cohete asignado.
- Gestión de suspensión y cancelación de lanzamientos por causas técnicas o económicas.
- Cobro y devolución de reservas mediante pasarela de pago ficticia.
- API REST y aplicación web interna para empleados.

### 2.2 Qué se excluye
- Venta pública online para clientes finales.
- Integraciones reales con pasarelas de pago, ERP o sistemas externos (solo pasarela ficticia para formación).
- Automatizaciones complejas (optimización predictiva de demanda o rutas).
- Requisitos de producción estrictos (alta disponibilidad, cumplimiento regulatorio real, hardening avanzado).

### 2.3 Limitaciones y notas
- Proyecto orientado a cursos y talleres; prioridad en claridad didáctica sobre complejidad real.
- Datos de ejemplo y empresa ficticia; no se contemplan datos productivos.
- MVP inicial sin seguridad, autenticación ni autorización.
- Se recomienda mantener contratos API y reglas de capacidad como núcleo del diseño para facilitar futuras especificaciones.
