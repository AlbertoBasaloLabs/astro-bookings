# TR2-rendimiento-interactivo-en-entorno-de-taller. Rendimiento interactivo en entorno de taller

## 1. Problema

### Contexto

Las operaciones de flota, calendario y reservas deben sentirse interactivas durante ejercicios de formación. El objetivo no es micro-optimización extrema, sino asegurar tiempos de respuesta estables sin comprometer reglas de negocio.

### Historias de usuario

- Como `operador` quiero **consultar flota y calendario con respuesta fluida** para _trabajar sin bloqueos perceptibles_.
- Como `administrativo` quiero **registrar reservas rápidamente** para _mantener ritmo operativo en taller_.
- Como `equipo-tecnico` quiero **priorizar consistencia sobre optimización prematura** para _evitar degradar reglas críticas_.

### Fuera de scope

- Benchmarks de escala masiva de producción.
- Autoescalado e infraestructura distribuida avanzada.
- Optimización agresiva que complejice el código didáctico.

---

## 2.Solución

Se establecen objetivos de latencia interactiva para operaciones principales y se monitoriza cumplimiento en pruebas de taller. Las decisiones de rendimiento no pueden romper validaciones de dominio ni atomicidad de reservas.

### Modelo


### Back

Mantener consultas y comandos principales con rutas de ejecución acotadas y sin N+1 evitables. Incorporar medición simple de duración por endpoint para detectar regresiones en operaciones críticas.

### Front

Optimizar interacción percibida con feedback inmediato (loading y confirmaciones) en acciones de consulta y reserva. Evitar recargas completas innecesarias en flujos frecuentes de taller.

---

## 3. Verificación

- [ ] el sistema DEBE mantener latencia interactiva en operaciones habituales de flota, calendario y reservas en entorno de taller.
- [ ] CUANDO una operación supere consistentemente el umbral definido el sistema DEBE reportar una señal de regresión de rendimiento.
- [ ] SI una optimización reduce latencia pero rompe una regla de negocio ENTONCES el sistema DEBE descartar esa optimización.
- [ ] CUANDO se ejecuten pruebas funcionales y de rendimiento el sistema DEBE priorizar consistencia de dominio como criterio de aceptación.
