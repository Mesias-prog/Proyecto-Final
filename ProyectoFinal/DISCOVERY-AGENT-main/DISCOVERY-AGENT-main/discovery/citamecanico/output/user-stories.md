# User Stories — citamecanico

> Generado el 2026-06-18 · Basado en `personas.md`, `requisitos.md` y `evidence-map.json`

---

## Núcleo del MVP

Las historias US-01 a US-05 cubren el valor mínimo entregable: eliminan los dolores más frecuentes y compartidos entre las tres personas primarias (saturación en la entrada, rampas muertas por falta de piezas y llamadas manuales de estatus).

---

- **[US-01]** Como administradora, quiero registrar y escalonar el ingreso de vehículos por franjas horarias para evitar que todos los clientes lleguen al mismo tiempo en la mañana y saturen el taller.
  - Criterios de aceptación:
    - Dado que la administradora recibe un vehículo, cuando asigna una franja horaria disponible en el sistema, entonces el carro queda registrado y el espacio de recepción de esa hora se descuenta.
    - Dado que una franja de ingreso alcanzó su capacidad máxima de vehículos, cuando se intenta agendar un nuevo carro en ese horario, entonces el sistema muestra una advertencia de saturación y sugiere la siguiente hora disponible.
  - Fuente: `cliente.md`, `recepcionista.md`

---

- **[US-02]** Como mecánico, quiero que el sistema controle el stock de repuestos críticos antes de iniciar un trabajo para no desarmar un motor y dejar el carro clavado ocupando una rampa por días.
  - Criterios de aceptación:
    - Dado que el mecánico va a registrar una orden de reparación, cuando selecciona los repuestos críticos necesarios, entonces el sistema valida contra el stock real y emite una alerta verde de disponibilidad o roja de desabastecimiento.
    - Dado que se confirma la orden con repuestos disponibles, cuando se inicia el trabajo, entonces esas piezas quedan reservadas automáticamente en el inventario digital para evitar que se asignen a otro carro.
  - Fuente: `recepcionista.md`

---

- **[US-03]** Como mecánico, quiero marcar una reparación como "trabajo terminado" en dos pasos desde un dispositivo en el taller para notificar de inmediato a la recepción sin tener que salir de la fosa.
  - Criterios de aceptación:
    - Dado que el mecánico finaliza el arreglo de un vehículo, cuando abre la orden desde la tablet o celular y presiona "Terminar Trabajo", entonces el estado de la rampa cambia a disponible.
    - Dado que el mecánico cambia el estado a terminado, cuando el sistema procesa la acción, entonces la administradora recibe una alerta visual en su pantalla y se genera de forma automática la pre-orden de facturación.
  - Fuente: `cliente.md`, `recepcionista.md`, `mecanico.md`

---

- **[US-04]** Como mecánico, quiero consultar el flujo de trabajos asignados y el orden de entrada desde mi celular fuera del taller para organizar de antemano las herramientas del día siguiente.
  - Criterios de aceptación:
    - Dado que el mecánico accede al sistema desde su dispositivo móvil personal, cuando abre su vista técnica, entonces ve la lista cronológica de carros asignados con modelo, placa, tipo de trabajo y estado de prioridad.
    - Dado que se añade o reprograma un ingreso en la administración, cuando ocurre el cambio, entonces la vista móvil del mecánico se actualiza automáticamente en tiempo real.
  - Fuente: `mecanico.md`

---

- **[US-05]** Como cliente, quiero recibir reportes de avance y evidencias visuales de las piezas dañadas por WhatsApp para aprobar presupuestos adicionales con total transparencia y confianza.
  - Criterios de aceptación:
    - Dado que el taller detecta un daño imprevisto y toma una foto de la pieza afectada, cuando la administradora adjunta la imagen al presupuesto digital, entonces el sistema facilita un enlace o mensaje estructurado de WhatsApp listo para enviar al cliente.
    - Dado que el cliente recibe el mensaje con la evidencia visual, cuando responde con su aprobación, entonces la administradora puede marcar el presupuesto extra como autorizado en la orden principal.
  - Fuente: `mecanico.md`, `recepcionista.md`

---

## Segunda fase (fuera del núcleo del MVP)

- **[US-06]** Como mecánico, quiero que el sistema me permita consultar un historial básico de fallas crónicas del vehículo al momento de ingresar para ir directo al grano sin gastar tiempo adivinando adaptaciones previas.
  - Criterios de aceptación:
    - Dado que un vehículo ingresa al taller, cuando el mecánico digita la placa en el sistema, entonces se despliega una tarjeta resumen con los últimos cambios de aceite, fechas de mantenimiento mayor y fallas recurrentes registradas.
    - Dado que el vehículo es completamente nuevo en el taller, cuando la administradora lo registra, entonces el sistema habilita un campo opcional de texto de 200 caracteres para anotar observaciones de mantenimiento previas declaradas por el dueño.
  - Fuente: `mecanico.md`
