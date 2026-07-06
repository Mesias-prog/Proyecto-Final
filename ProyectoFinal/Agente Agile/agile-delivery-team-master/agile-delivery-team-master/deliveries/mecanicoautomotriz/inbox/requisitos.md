# Requisitos Candidatos — tallermecanico

> Generado el 2026-07-04 · Fuentes: `asesor.md`, `mecanico.md`, `cliente.md`

---

## Funcionales

- **[R-01]** El sistema debe permitir que los clientes agenden citas para mantenimiento o reparación en línea, fuera del horario laboral del taller.
  - Tipo: funcional
  - Origen: `asesor.md` · Asesor de Servicio / `cliente.md` · Cliente

- **[R-02]** El sistema debe validar la disponibilidad de bahías de trabajo en tiempo real e impedir que se asigne un vehículo a una bahía ocupada.
  - Tipo: funcional
  - Origen: `asesor.md` · Asesor de Servicio

- **[R-03]** El sistema debe enviar notificaciones automáticas a los clientes (por WhatsApp o canal equivalente) sobre el estado de su vehículo, sin intervención manual del asesor.
  - Tipo: funcional
  - Origen: `asesor.md` · Asesor de Servicio / `cliente.md` · Cliente

- **[R-04]** El mecánico (jefe de taller) debe poder consultar y gestionar sus órdenes de trabajo diarias desde cualquier dispositivo, sin necesidad de ir a la oficina.
  - Tipo: funcional
  - Origen: `mecanico.md` · Mecánico

- **[R-05]** Al momento de agendar, el cliente debe poder registrar una descripción breve de la falla o el tipo de mantenimiento requerido, visible para el mecánico antes de recibir el vehículo.
  - Tipo: funcional
  - Origen: `mecanico.md` · Mecánico

- **[R-06]** El sistema debe exponer la disponibilidad de bahías de forma que los clientes puedan consultar espacios libres sin necesidad de llamar por teléfono.
  - Tipo: funcional
  - Origen: `asesor.md` · Asesor de Servicio / `cliente.md` · Cliente

- **[R-07]** El jefe de taller debe poder bloquear bahías (por mantenimiento de equipo, ausencias del personal o imprevistos) y dichos bloqueos deben reflejarse de inmediato en la agenda.
  - Tipo: funcional
  - Origen: `mecanico.md` · Mecánico / `asesor.md` · Asesor de Servicio

---

## No funcionales

- **[R-08]** El sistema debe ser accesible desde dispositivos móviles (celular), tanto para el mecánico en el piso de operaciones como para los clientes.
  - Tipo: no funcional
  - Origen: `mecanico.md` · Mecánico / `cliente.md` · Cliente

- **[R-09]** El sistema debe estar disponible las 24 horas del día para la consulta de disponibilidad y el agendamiento, independientemente del horario de apertura del taller.
  - Tipo: no funcional
  - Origen: `cliente.md` · Cliente
