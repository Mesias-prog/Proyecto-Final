# ADR-0003 · Base de datos relacional (PostgreSQL) y bloqueo pesimista para concurrencia

**Estado:** aceptado
**Fecha:** 2026-07-04

---

## Contexto y fuerza

**US-02** establece el criterio de aceptación más estricto del MVP desde el punto de vista de integridad de datos:

> "Dado que dos solicitudes de cita coinciden en la misma bahía y horario, cuando la primera se confirma, **entonces la segunda recibe la bahía como bloqueada antes de finalizar**."

`requisitos.md` req:R-02: "El sistema debe validar la disponibilidad de bahías de trabajo en tiempo real e impedir que se asigne un vehículo a una bahía ocupada."

`personas.md` / `asesor.md` pain `sobreagendamiento-bahias`: ocurren constantemente debido a la desincronización entre el cuaderno y el Excel, lo que genera cuellos de botella operativos graves en el taller.

La fuerza es que **la consistencia ante escrituras concurrentes sobre la misma bahía es no negociable**. No es un requisito de rendimiento extremo (el taller no tiene miles de reservas simultáneas), sino un requisito de correctitud operativa para evitar el caos en el taller.

---

## Decisión

Se adopta **PostgreSQL** como única base de datos del sistema, con **bloqueo pesimista a nivel de fila** (`SELECT FOR UPDATE`) dentro de una transacción para la operación de agendamiento:

```sql
-- Dentro de una transacción
SELECT id, estado FROM bahias
WHERE id = :bahia_id AND estado = 'disponible'
FOR UPDATE;

-- Si devuelve fila: actualizar estado a 'reservada' y hacer COMMIT
-- Si no devuelve fila (ya estaba ocupada): ROLLBACK y devolver error 409
```

Este mecanismo garantiza que dos peticiones concurrentes sobre la misma franja no pueden ambas leer `disponible` y luego ambas escribir `reservada`; la segunda espera a que la primera libere el bloqueo y entonces lee el estado ya actualizado.

Todas las entidades del dominio (citas, bahias, bloqueos, pacientes) se modelan en el mismo esquema relacional de PostgreSQL.

---

## Alternativas consideradas

- **Base de datos NoSQL (MongoDB, DynamoDB u equivalente)** — Los modelos de documento sin transacciones ACID nativas no garantizan la consistencia requerida por US-02 ante escrituras concurrentes. Aunque algunos NoSQL ofrecen transacciones, añaden complejidad de configuración sin ventaja real para un dominio estructurado como el de un taller. `Descartado`.
- **Bloqueo optimista (versión de fila / ETag)** — Más eficiente en escenarios de baja contención, pero requiere lógica de reintento en la capa de aplicación. Para el volumen de un taller pequeño, la lógica de reintento no aporta beneficio y añade complejidad innecesaria. El bloqueo pesimista es más simple y correcto. `Descartado`.
- **Bloqueo en memoria (Redis, lock de aplicación)** — Introduce un punto de fallo adicional (servidor de caché) y complejidad de sincronización. No hay evidencia en el inbox de que el volumen actual requiera este nivel de rendimiento. `Descartado`.
- **SQLite** — Adecuado solo para desarrollo local, no es apto para entornos de producción con acceso concurrente. `Descartado` para producción.

---

## Consecuencias

**Lo que se gana:**
- Correctitud garantizada por el motor de base de datos: US-02 se cumple por diseño.
- Modelo relacional alineado con el dominio: bahías, citas y bloqueos tienen relaciones claras que el esquema relacional expresa naturalmente.
- PostgreSQL es maduro, ampliamente soportado y escalable según las necesidades futuras del taller. (OQ-04 en `architecture.md`).

**El costo que se acepta:**
- El bloqueo pesimista introduce una espera breve (milisegundos) si dos usuarios reservan la misma bahía exactamente al mismo tiempo, lo cual es totalmente aceptable.
- Requiere que el equipo comprenda las garantías transaccionales de PostgreSQL.
- Si en el futuro el esquema necesita cambios (migración), se requiere gestión de migraciones (por ejemplo: Flyway, Alembic, o equivalente según el lenguaje elegido en OQ-01).
