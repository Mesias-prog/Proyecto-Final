# Historias de usuario refinadas — Taller Mecánico MVP
> Refinadas el 2026-07-04 · Developer del equipo ágil · Todas las historias cumplen INVEST + Definition of Ready

---

## E-01 · Agendamiento de bahías sin llamadas

### US-01 · Reserva de turno de bahía online 24/7   ·   épica E-01   ·   8 pts
**Como** cliente, **quiero** agendar un turno de servicio en línea en cualquier momento del día, **para** no tener que depender del horario de atención telefónica del taller.

Criterios de aceptacion (Gherkin):
- Dado que el cliente accede al sistema, cuando elige fecha, hora y bahía disponible, y confirma, entonces la reserva queda registrada y recibe confirmación por WhatsApp.
- Dado que el cliente intenta seleccionar una bahía ocupada, cuando intenta confirmarla, entonces el sistema la muestra como no disponible y lo invita a elegir otra.

Origen: us:US-01 · req:R-01 · req:R-06 · evidence:cliente.md · evidence:asesor.md

---

### US-02 · Prevencion de doble ocupación de bahías   ·   épica E-01   ·   5 pts
**Como** asesor, **quiero** que el sistema impida asignar la misma bahía a dos vehículos de forma simultánea, **para** evitar cuellos de botella operativos y conflictos con clientes que llegan y no tienen espacio.

Criterios de aceptacion (Gherkin):
- Dado que dos solicitudes de reserva coinciden en la misma bahía, cuando la primera se confirma, entonces la segunda recibe la bahía como bloqueada antes de finalizar su proceso de reserva.
- Dado que existe una orden activa en una bahía, cuando se intenta agregar otra orden en el mismo horario, entonces el sistema rechaza la operación con un mensaje de conflicto claro.

Origen: us:US-02 · req:R-02 · evidence:asesor.md

---

## E-02 · Notificaciones automáticas de estado

### US-03 · Notificación automática por WhatsApp con estado del vehículo   ·   épica E-02   ·   8 pts
**Como** cliente, **quiero** recibir notificaciones automáticas por WhatsApp sobre el estado de mi reparación (revisión, listo para retiro), **para** saber cuándo mi vehículo está disponible sin necesidad de llamar al taller.

Criterios de aceptacion (Gherkin):
- Dado que un vehículo cambia de estado (ej. 'reparación iniciada', 'listo para retiro'), cuando el jefe de taller actualiza el sistema, entonces el cliente recibe un WhatsApp automático con la novedad.
- Dado que el cliente recibe la notificación de listo, cuando responde confirmando su llegada, entonces el sistema alerta al asesor.

Origen: us:US-03 · req:R-03 · evidence:asesor.md · evidence:cliente.md

> **Supuestos asumidos:** (1) El costo de la API de WhatsApp Business está dentro del presupuesto del taller (confirmación pendiente con el dueño). (2) Tasa de adopción suficiente para la base de clientes actual.

---

## E-03 · Control operativo de órdenes y bahías

### US-04 · Consulta de órdenes desde celular en tiempo real   ·   épica E-03   ·   3 pts
**Como** jefe de taller, **quiero** consultar mis órdenes de trabajo desde el celular con actualización en tiempo real, **para** trabajar en el piso de operaciones sin ir a la oficina a preguntar qué autos tengo pendientes.

Criterios de aceptacion (Gherkin):
- Dado que el jefe de taller accede desde el móvil, cuando abre la app, entonces ve la lista de vehículos, estado y bahía asignada.
- Dado que se asigna una nueva orden o cambia su estado, cuando ocurre el cambio, entonces la lista se actualiza automáticamente (polling) sin necesidad de recargar la página.

Origen: us:US-04 · req:R-04 · req:R-08 · evidence:mecanico.md

---

### US-05 · Bloqueo de bahías por mantenimiento   ·   épica E-03   ·   3 pts
**Como** jefe de taller, **quiero** bloquear bahías (por mantenimiento o imprevistos), **para** que los clientes no puedan agendar servicios cuando la bahía no está operativa, evitando reasignaciones de último momento.

Criterios de aceptacion (Gherkin):
- Dado que selecciono una bahía para bloquear, cuando confirmo el bloqueo, entonces esa bahía desaparece de la disponibilidad pública de inmediato.
- Dado que hay un bloqueo activo, cuando un cliente intenta agendar, entonces el sistema no le permite seleccionar esa bahía ni ninguna subfranja dentro de ella.

Origen: us:US-05 · req:R-07 · evidence:mecanico.md · evidence:asesor.md

---

## E-04 · Diagnóstico previo (segunda fase)

### US-06 · Reporte de falla al agendar   ·   épica E-04   ·   2 pts

> ADVERTENCIA Segunda fase — fuera del alcance del Sprint 1 (mvp-canvas.md). No es el cuello de botella actual (sobreagendamiento). Se documenta para planificación futura.

**Como** cliente, **quiero** indicar brevemente la falla de mi vehículo al agendar, **para** que el taller esté preparado con el equipo o repuestos necesarios al recibir el auto.

Criterios de aceptacion (Gherkin):
- Dado que el cliente reserva, cuando llega al paso de confirmación, entonces puede escribir la falla (campo opcional, máximo 200 caracteres).
- Dado que la orden se crea, cuando el jefe de taller la revisa, entonces ve el detalle de la falla reportada antes de que llegue el vehículo.

Origen: us:US-06 · req:R-05 · evidence:mecanico.md

---

## Resumen del backlog refinado

| ID | Título corto | Épica | Pts | Prioridad | Estado DoR |
|-------|-------------------------------------------|-------|-----|-----------|------------|
| US-01 | Reserva de turno online 24/7 | E-01 | 8 | 1 | Lista |
| US-02 | Prevención de doble ocupación | E-01 | 5 | 2 | Lista |
| US-03 | Notificación WhatsApp automática | E-02 | 8 | 3 | Lista |
| US-04 | Consulta de órdenes desde celular | E-03 | 3 | 4 | Lista |
| US-05 | Bloqueo de bahías | E-03 | 3 | 5 | Lista |
| US-06 | Reporte de falla (fase 2) | E-04 | 2 | 6 | Lista (fuera Sprint 1) |

**Total Sprint 1 candidato (US-01 a US-05):** 27 pts