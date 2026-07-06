# ADR-0001 · Arquitectura monolítica modular

**Estado:** aceptado
**Fecha:** 2026-07-04

---

## Contexto y fuerza

El taller mecánico es una operación pequeña que cuenta con **un asesor de servicio, un jefe de taller (mecánico) y una base de clientes recurrentes** (`personas.md`). El MVP cubre exactamente cinco historias de usuario (US-01 a US-05) con un enfoque en la eficiencia operativa del taller. No hay evidencia en el inbox de requisitos de alta escalabilidad horizontal, múltiples equipos de desarrollo desplegando en paralelo, ni dominios técnicos que requieran despliegues independientes.

La fuerza que motiva esta decisión es la **escala inicial del negocio** (`mvp-canvas.md` — segmento: taller pequeño) y el **principio rector de la arquitectura** (máxima simplicidad operativa). Añadir microservicios introduciría una complejidad innecesaria (orquestación, red, trazabilidad distribuida) que el contexto operativo del taller no justifica actualmente.
---

## Decisión

Se adopta un **monolito modular**: una sola unidad desplegable que contiene tres módulos internos con fronteras bien definidas:

- **Módulo Agendamiento** — gestión de citas, disponibilidad de bahías y validación de horarios (US-01, US-02, US-05)
- **Módulo Operativo** — gestión de órdenes de trabajo diarias y estado de los vehículos (US-04)
- **Módulo Notificaciones** — comunicación automatizada por WhatsApp sobre el estado de las reparaciones (US-03)

Los módulos se comunican mediante llamadas directas en memoria, comparten la misma base de datos y se despliegan como una sola unidad.
---

## Alternativas consideradas

- **Microservicios (un servicio por módulo)** — Añade latencia de red, complejidad de despliegue y necesidad de infraestructura adicional (service mesh, gateway). No hay un equipo grande ni necesidades de escalabilidad que lo justifiquen. `Descartado`.
- **Monolito sin separación de módulos (big ball of mud)** — Más rápido al inicio, pero dificulta la futura extracción si el negocio crece (por ejemplo, si se añaden múltiples sucursales en fase 2). `Descartado` para preservar la capacidad de evolución del sistema.
- **Arquitectura serverless (funciones)** — Apropiada para cargas de trabajo altamente impredecibles. El volumen de un taller pequeño es estable y predecible, por lo que la complejidad de orquestación no se justifica. `Descartado`.

---

## Consecuencias

**Lo que se gana:**
- Despliegue simple: un solo proceso y un único punto de monitoreo.
- Menor tiempo de llegada a producción para el MVP.
- Las fronteras modulares permiten extraer un servicio en el futuro (ej. el módulo de notificaciones) sin reescribir la lógica central del taller.

**El costo que se acepta:**
- El escalamiento horizontal implica replicar el monolito completo (aceptable dado el volumen actual del taller).
- Un fallo grave en un módulo puede comprometer a los demás (mitigado con pruebas modulares robustas y revisiones de código).
- Si el negocio crece significativamente antes de la segunda fase, la deuda de separar módulos aumentará. Se revisará esta decisión al incorporar múltiples sucursales o al superar la capacidad de una sola instancia de servidor.