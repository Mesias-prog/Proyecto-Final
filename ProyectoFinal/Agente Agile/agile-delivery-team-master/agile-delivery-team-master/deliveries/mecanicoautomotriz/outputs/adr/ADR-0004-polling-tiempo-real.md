# ADR-0004 · Estrategia de actualización en tiempo real: polling periódico con ruta de escalado a SSE

**Estado:** aceptado
**Fecha:** 2026-07-04

---

## Contexto y fuerza

**US-04** y **US-05** requieren visibilidad inmediata de los cambios en el taller:

- US-04, criterio 2: "Cuando se actualiza el estado de una reparación, entonces la vista del mecánico se actualiza sin necesidad de recargar manualmente."
- US-05, criterio 1: "Cuando el mecánico confirma el bloqueo de bahía, entonces esas bahías desaparecen de la disponibilidad pública de inmediato."
- `requisitos.md` req:R-04: "El mecánico debe poder consultar sus órdenes de trabajo desde cualquier dispositivo, sin necesidad de ir a la oficina."
- req:R-07: "Los bloqueos de bahías deben reflejarse de inmediato para todos."

El equipo ha señalado que la estrategia de actualización afecta directamente la complejidad de implementación y la validación de las estimaciones del sprint (US-04 y US-05). El contexto es un **mecánico consultando órdenes desde el celular en el piso de operaciones** (`personas.md` / `mecanico.md`). No hay evidencia de múltiples mecánicos trabajando en paralelo con alta frecuencia de actualización de estados.

---

## Decisión

Se implementa **polling HTTP periódico cada 15 segundos** desde el cliente (vista del mecánico/asesor) al endpoint `GET /ordenes?fecha=<fecha>`.

- La latencia máxima de actualización es de 15 segundos: cualquier cambio en el estado de una orden será visible para el mecánico en el próximo ciclo de polling.
- Para los bloqueos de bahías (US-05): el bloqueo se persiste de forma síncrona al confirmar (`POST /bloqueos`). Desde ese momento, el endpoint de disponibilidad pública (`GET /bahias`) ya no devuelve ese espacio. El mecánico ve su propio bloqueo confirmado inmediatamente por la respuesta exitosa del POST; otros usuarios (asesor) verán el bloqueo en su próximo ciclo de polling.
- Si el feedback post-MVP muestra que 15 segundos de latencia afectan la eficiencia operativa, se escala a **Server-Sent Events (SSE)**: el servidor mantiene una conexión unidireccional y envía eventos al cliente al detectar cambios. Esto no requiere cambios en la lógica de negocio ni en la base de datos, solo en la capa de transporte API.

---

## Alternativas consideradas

- **WebSockets** — Permite tiempo real con latencia casi nula, pero requiere infraestructura adicional (servidor con soporte persistente, gestión de estados de conexión, proxies). No hay evidencia de que una latencia de 15 segundos represente un dolor operativo. Añade complejidad injustificada. `Descartado` para el MVP.
- **Server-Sent Events (SSE) desde el inicio** — Es la ruta natural de escalado, pero añade gestión de conexiones abiertas y reconexión automática en el cliente que no es necesaria hasta validar el dolor operativo. `Descartado` para el MVP, pero es la ruta de escalado inmediata si el polling resulta insuficiente.
- **Polling con intervalo corto (1-2 segundos)** — Generaría carga innecesaria en el servidor sin un beneficio documentado. `Descartado`; 15 segundos es el punto de equilibrio óptimo inicial (OQ-05 en `architecture.md`).
- **Long polling** — Más complejo de implementar que el polling simple sin ofrecer ventajas significativas sobre SSE o polling estándar. `Descartado`.

---

## Consecuencias

**Lo que se gana:**
- Implementación simple: endpoint GET estándar, sin infraestructura de red compleja. Las estimaciones de esfuerzo para US-04 y US-05 se mantienen seguras.
- Sin dependencias de librerías externas o proxies específicos; compatibilidad total con cualquier framework HTTP.
- Ruta de escalado clara: pasar a SSE es una mejora puramente técnica sin impacto en la lógica de negocio.

**El costo que se acepta:**
- Latencia máxima de 15 segundos en la vista de órdenes del mecánico. Nota: cualquier acción realizada por el propio mecánico (ej. cambiar estado de una orden) se refleja localmente de inmediato al recibir el OK del servidor, mitigando la sensación de espera.
- La decisión de escalar a SSE debe evaluarse al finalizar la fase 1, basándose en la experiencia directa del mecánico en el taller.