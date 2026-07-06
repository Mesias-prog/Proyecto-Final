# User Stories — tallermecanico

> Generado el 2026-07-04 · Basado en `personas.md`, `requisitos.md` y `evidence-map.json`

---

## Núcleo del MVP

Las historias US-01 a US-05 cubren el valor mínimo entregable: eliminan los cuellos de botella operativos y la fricción de comunicación entre clientes, asesor y mecánico.

---

- **[US-01]** Como cliente, quiero agendar una cita de mantenimiento en línea en cualquier momento para no tener que llamar durante mi jornada laboral ni enfrentar líneas ocupadas.
  - Criterios de aceptación:
    - Dado que el cliente accede al sistema fuera del horario del taller, cuando elige bahía, fecha y hora disponibles y confirma, entonces la cita queda registrada y recibe confirmación por WhatsApp.
    - Dado que el cliente intenta seleccionar una bahía ya ocupada, cuando intenta confirmarla, entonces el sistema la muestra como no disponible y lo invita a elegir otra.
  - Fuente: `cliente.md`, `asesor.md`

---

- **[US-02]** Como asesor de servicio, quiero que el sistema impida asignar el mismo vehículo a una bahía ya ocupada para evitar cuellos de botella operativos en el taller.
  - Criterios de aceptación:
    - Dado que dos solicitudes de cita coinciden en la misma bahía y horario, cuando la primera se confirma, entonces la segunda recibe el slot como bloqueado antes de finalizar.
    - Dado que existe un vehículo asignado a una bahía, cuando se intenta agregar otro en el mismo horario, entonces el sistema rechaza la operación con un mensaje de conflicto.
  - Fuente: `asesor.md`

---

- **[US-03]** Como cliente, quiero recibir notificaciones automáticas por WhatsApp sobre el estado de mi vehículo para saber cuándo está listo sin tener que llamar al taller.
  - Criterios de aceptación:
    - Dado que un vehículo tiene una cita activa, cuando el estado cambia a "listo para entrega", entonces el sistema envía un mensaje de WhatsApp con la fecha, hora y confirmación de finalización.
    - Dado que el cliente recibe la notificación, cuando responde confirmando el retiro, entonces el sistema marca la orden como "en espera de retiro" para el mecánico.
  - Fuente: `cliente.md`, `asesor.md`, `mecanico.md`

---

- **[US-04]** Como mecánico, quiero consultar mis órdenes de trabajo diarias desde mi celular para no tener que ir a la oficina constantemente a preguntar qué sigue.
  - Criterios de aceptación:
    - Dado que el mecánico accede desde un dispositivo móvil, cuando abre su vista de órdenes, entonces ve la lista de vehículos del día con modelo, falla reportada y estado (en revisión / reparando / listo).
    - Dado que se actualiza el estado de una reparación, cuando ocurre el cambio, entonces la vista del mecánico se actualiza sin necesidad de recargar manualmente.
  - Fuente: `mecanico.md`

---
- **[US-05]** Como mecánico, quiero bloquear bahías por mantenimiento de equipos o ausencias del personal para que los clientes no puedan agendar en esos períodos.
  - Criterios de aceptación:
    - Dado que el mecánico selecciona un rango de fechas u horas para bloquear, cuando confirma, entonces esas bahías desaparecen de la disponibilidad pública de inmediato.
    - Dado que hay un bloqueo activo, cuando un cliente intenta agendar en ese período, entonces el sistema no le ofrece esa bahía ni ninguna subfranja dentro de ella.
  - Fuente: `mecanico.md`, `asesor.md`

---

## Segunda fase (fuera del núcleo del MVP)

- **[US-06]** Como mecánico, quiero que el cliente describa la falla al agendar para aprovechar mejor el tiempo de diagnóstico y no perder minutos averiguando lo básico.
  - Criterios de aceptación:
    - Dado que el cliente completa el formulario de agendamiento, cuando llega al paso de confirmación, entonces puede escribir la descripción de la falla (campo opcional, máx. 200 caracteres).
    - Dado que la cita existe, cuando el mecánico revisa su orden del día, entonces puede ver la descripción del problema junto al modelo del vehículo antes de que este llegue al taller.
  - Fuente: `mecanico.md`