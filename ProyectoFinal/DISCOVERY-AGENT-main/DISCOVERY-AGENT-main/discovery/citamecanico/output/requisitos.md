# Requisitos Candidatos — citamecanico

> Generado el 2026-06-18 · Fuentes: `recepcionista.md`, `mecanico.md`, `cliente.md`

---

## Funcionales

- **[R-01]** El sistema debe permitir el agendamiento y escalonamiento digital de citas de ingreso de vehículos por franjas horarias, evitando la acumulación de carros en la mañana.
  - Tipo: funcional
  - Origen: `recepcionista.md` · Recepcionista / `cliente.md` · cliente

- **[R-02]** El sistema debe permitir el registro y control básico en tiempo real del stock de repuestos críticos, alertando sobre la disponibilidad antes de iniciar una reparación.
  - Tipo: funcional
  - Origen: `recepcionista.md` · Recepcionista

- **[R-03]** El sistema debe permitir al mecánico cambiar el estado de una reparación a "trabajo terminado" desde el taller, generando de forma automática la pre-orden de facturación en la recepción.
  - Tipo: funcional
  - Origen: `recepcionista.md` · Recepcionista / `cliente.md` · cliente

- **[R-04]** El mecánico debe poder consultar el flujo de trabajos asignados y el orden de entrada de vehículos desde cualquier dispositivo fuera del taller.
  - Tipo: funcional
  - Origen: `mecanico.md` · mecanico

- **[R-05]** Al momento del ingreso del vehículo, el sistema debe permitir adjuntar o consultar un historial básico de reparaciones previas o fallas crónicas del carro.
  - Tipo: funcional
  - Origen: `mecanico.md` · mecanico

- **[R-06]** El sistema debe facilitar el envío manual o semiautomatizado de reportes de avance, presupuestos y evidencias visuales (fotos/videos de piezas dañadas) al cliente vía WhatsApp.
  - Tipo: funcional
  - Origen: `recepcionista.md` · Recepcionista / `cliente.md` · cliente

- **[R-07]** El sistema debe emitir alertas o notificaciones de cumplimiento sobre las fechas de entrega pactadas con el cliente para mitigar retrasos en el taller.
  - Tipo: funcional
  - Origen: `mecanico.md` · Mécanico / `recepcionista.md` · Recepcionista

---

## No funcionales

- **[R-08]** El sistema debe ser accesible desde dispositivos móviles (celular), tanto para la médico como para los pacientes.
  - Tipo: no funcional
  - Origen: `mecanico.md` · mecanico / `cliente.md` · clietne

- **[R-09]** La consulta del estado de la reparación y la visualización del flujo técnico de la agenda de rampas debe estar disponible en tiempo real las 24 horas del día.
  - Tipo: no funcional
  - Origen: `mecanico.md` ·Mecánico/ `cliente.md` · Paciente
