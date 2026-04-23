# TR3-confiabilidad. 

## 1. Problema

Confiabilidad

### Contexto

La confiabilidad del MVP depende de evitar sobreventa y mantener integridad de estados frente a fallos intermedios. Sin ejecución atómica y errores consistentes, el sistema puede quedar en estados imposibles de auditar o corregir.

### Historias de usuario

- Como `operaciones` quiero **crear reservas sin riesgo de sobreventa** para _preservar capacidad real_.
- Como `soporte` quiero **recibir errores de dominio consistentes** para _resolver incidencias rápidamente_.
- Como `arquitectura` quiero **evitar estados parciales ante fallos** para _garantizar integridad del sistema_.

### Fuera de scope

- Estrategias de recuperación multi-región.
- Garantías de exactly-once en integraciones externas reales.
- Tolerancia a fallos de infraestructura de nivel enterprise.

---

## 2.Solución

Se exige atomicidad en creación de reservas y transiciones críticas de estado mediante transacciones y control de concurrencia. Los errores de negocio se normalizan en un contrato estable con códigos y mensajes deterministas.

### Modelo


### Back

Implementar transacciones en comandos críticos y rollback automático ante excepción de negocio o infraestructura. Añadir capa de mapeo de errores de dominio para homogeneizar respuestas API.

### Front

Consumir códigos de error de dominio para mostrar mensajes claros y accionables. Evitar asumir éxito optimista en operaciones críticas hasta confirmación backend.

---

## 3. Verificación

- [ ] el sistema DEBE ejecutar creación de reservas de forma atómica.
- [ ] CUANDO ocurra un fallo intermedio en una operación crítica el sistema DEBE revertir completamente los cambios.
- [ ] SI una regla de dominio se incumple ENTONCES el sistema DEBE devolver un error explícito y consistente.
- [ ] CUANDO se inspeccione el estado posterior a un error el sistema DEBE conservar integridad de datos y ciclo de vida.
