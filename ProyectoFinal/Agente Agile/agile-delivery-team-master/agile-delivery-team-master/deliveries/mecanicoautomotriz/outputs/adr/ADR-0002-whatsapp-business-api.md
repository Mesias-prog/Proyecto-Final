# ADR-0002 · WhatsApp Business API como canal de notificaciones

**Estado:** aceptado
**Fecha:** 2026-07-04

---

## Contexto y fuerza

El inbox provee evidencia directa de que **WhatsApp es el canal preferido del cliente**:

- `personas.md` / `cliente.md`: C. (cliente) prefiere WhatsApp para coordinar citas, ya que el agendamiento telefónico es lento y la línea suele estar ocupada. 
- `mvp-canvas.md` — Funcionalidades mínimas F1 y F3: "Agendamiento online vía WhatsApp" y "Notificación automática por WhatsApp".
- `requisitos.md` req:R-03: "El sistema debe enviar notificaciones automáticas a los clientes (por WhatsApp o canal equivalente)".
- `evidence-map.json` pains: `agendamiento-telefonico-lento` (cliente.md), `notificaciones-manuales-estado` (asesor.md).

La fuerza directa es **req:R-03 + personas.md (C.)**: sin un canal confiable de notificación automatizada, el asesor de servicio seguirá invirtiendo tiempo en llamadas de consulta (dolor registrado) y el cliente permanecerá en la incertidumbre sobre su vehículo.

**Supuesto de riesgo explícito registrado en el inbox:** `mvp-canvas.md` riesgo 4: "El costo de la integración/API de WhatsApp es asumible por el taller" — marcado como supuesto, no como hecho confirmado.---

## Decisión

Se utiliza **WhatsApp Business API a través de un proveedor habilitado por Meta** (por ejemplo: Twilio, MessageBird, u otro equivalente) para el envío de:

1. Confirmación de cita de mantenimiento (US-01).
2. Notificación automática de cambio de estado del vehículo (en revisión, reparando, listo) (US-03).
3. Confirmación de disponibilidad para retiro del vehículo.

La integración se encapsula dentro del **Módulo Notificaciones** (ver ADR-0001), de modo que si en el futuro se cambia de proveedor, el impacto queda contenido en ese módulo.

El proveedor concreto (Twilio vs Meta directa vs otro) queda como **OQ-02** en `architecture.md` porque el costo no está confirmado y la aprobación de cuenta tiene plazos variables. Esta decisión no bloquea el desarrollo del resto del MVP.

---

## Alternativas consideradas

- **SMS (Twilio SMS)** — Mayor alcance, pero costo por mensaje a veces superior y sin capacidad de mostrar "estados" de manera tan natural como WhatsApp. La evidencia del inbox apunta a WhatsApp como el canal de uso común para el cliente. `Descartado` como canal primario.
- **Correo electrónico** — No hay mención en el inbox de que los clientes usen email para interactuar con el taller. Introducirlo sería una invención sin respaldo. `Descartado`.
- **Notificación push (App nativa)** — Requeriría que los clientes instalaran una aplicación, lo que agrega fricción y un vector de adopción no respaldado por la evidencia de preferencia del cliente. `Descartado`.
- **Llamadas telefónicas manuales** — Es el canal actual que causa el dolor `llamadas-estado-vehiculo`. Automatizar llamadas (IVR) tampoco resuelve la preferencia de respuesta asíncrona del cliente. `Descartado`.
---

## Consecuencias

**Lo que se gana:**
- Se atiende el dolor documentado de C. y el del asesor (A.) mediante el canal que ambos ya utilizan.
- La API permite interacciones clave para mantener al cliente informado sin intervención manual constante.
- El Módulo Notificaciones queda desacoplado del resto del sistema.

**El costo que se acepta:**
- **Riesgo de costo no confirmado**: la API de WhatsApp tiene costos por conversación (Meta). Si el dueño del taller rechaza el costo, se debe renegociar el canal antes de iniciar US-03. Este riesgo está registrado en `mvp-canvas.md` riesgo 4.
- Dependencia de un servicio externo: si el proveedor tiene una interrupción, las notificaciones no se envían. Mitigación: reintentos automáticos en el Módulo Notificaciones.
- Proceso de aprobación en Meta: requiere gestionar la cuenta de negocio, lo cual debe iniciarse en paralelo al desarrollo.