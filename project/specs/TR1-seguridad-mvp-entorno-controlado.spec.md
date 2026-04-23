# TR1-seguridad-mvp-entorno-controlado. Seguridad MVP en entorno controlado

## 1. Problema

### Contexto

El PRD define explícitamente que el MVP no incluye autenticación ni autorización por tratarse de entorno de taller controlado. Aun así, hay que documentar límites para no generar una falsa sensación de seguridad ni bloquear evolución futura.

### Historias de usuario

- Como `formador` quiero **operar el sistema sin fricción de login** para _centrar el taller en reglas de negocio_.
- Como `equipo-tecnico` quiero **dejar explícitas las exclusiones de seguridad** para _evitar maluso del MVP en producción_.
- Como `arquitecto` quiero **preservar puntos de extensión para seguridad futura** para _facilitar hardening posterior_.

### Fuera de scope

- Implementar autenticación de usuarios.
- Implementar autorización por roles/permisos.
- Cumplimiento regulatorio de seguridad productiva.

---

## 2.Solución

Se define una política explícita de seguridad mínima para entorno de taller: acceso abierto a funcionalidades internas, sin credenciales ni control de permisos. Se documentan fronteras y contratos preparados para introducir capa de identidad en iteraciones futuras.

### Modelo

#### Politica-seguridad-mvp
- modo-seguridad: `enum(taller-controlado)`, modo operativo del entorno.
- autenticacion-habilitada: `boolean=false`, bandera fija para MVP.
- autorizacion-habilitada: `boolean=false`, bandera fija para MVP.
- aviso-uso: `string`, leyenda de no apto para producción.
- Regla1: todo endpoint interno opera sin requerir credenciales en MVP.
- Regla2: la documentación DEBE indicar que seguridad avanzada está diferida.

### Back

No se incluye middleware de authn/authz. Mantener estructura de enrutado que permita insertar middleware en borde API sin ruptura de contratos funcionales existentes.

### Front

No hay flujo de login. Mostrar en layout interno aviso de contexto formativo/no productivo para evitar interpretación incorrecta del alcance de seguridad.

---

## 3. Verificación

- [ ] el sistema DEBE operar sin autenticación ni autorización en modo taller.
- [ ] CUANDO se revise documentación técnica el sistema DEBE declarar explícitamente que seguridad avanzada no forma parte del MVP.
- [ ] SI se intenta usar el sistema como entorno productivo ENTONCES el sistema DEBE considerarse fuera de alcance y no conforme a TR1.
- [ ] CUANDO se diseñen nuevos endpoints el sistema DEBE mantener puntos de extensión para añadir middleware de seguridad futuro.
