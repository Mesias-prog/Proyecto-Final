# ADR-0005 · Autenticación por usuario y contraseña para acceso interno (asesor y mecánico)

**Estado:** aceptado
**Fecha:** 2026-07-04

---

## Contexto y fuerza

El MVP incluye dos vistas diferenciadas según el rol del usuario:

- **Vista pública (cliente):** acceso sin autenticación; el cliente solo proporciona la información mínima al reservar (US-01). No hay cuentas de usuario para clientes en el MVP.
- **Vista interna (asesor / mecánico):** acceso a datos operativos sensibles, agenda del día, historial de fallas y bloqueo de bahías (US-04, US-05). Esta vista debe estar protegida.

`requisitos.md` req:R-04: "El mecánico debe poder consultar sus órdenes desde cualquier dispositivo." Esto implica que la autenticación debe funcionar desde un celular sin fricción constante.

`personas.md` / `mecanico.md`: Carlos (jefe de taller) necesita acceder a sus órdenes desde el piso de operaciones, fuera de la oficina.

No existe un requisito de autenticación explícito en el inbox, pero proteger la vista interna es una consecuencia lógica de req:R-04 y req:R-07: permitir acceso libre a la gestión de órdenes de trabajo comprometería la integridad operativa y la gestión de citas.

---

## Decisión

Se implementa **autenticación con usuario y contraseña** para la vista interna, con las siguientes características:

- Dos usuarios iniciales configurados: uno para el asesor de servicio (A.) y uno para el jefe de taller (Carlos).
- El backend valida credenciales y emite un **token JWT** con expiración (ej. 8 horas), almacenado por el frontend en memoria o en una cookie HTTP-only segura.
- Las rutas del API internas (`/ordenes`, `/bloqueos`, `/bahias`) requieren el token válido.
- La gestión de usuarios (creación, cambio de clave) queda fuera del MVP; se configura en el despliegue inicial.

---

## Alternativas consideradas

- **Magic Link (enlace por WhatsApp)** — Aunque WhatsApp es el canal preferido, usarlo como mecanismo de autenticación para el personal interno crea una dependencia crítica. Si WhatsApp falla, el personal no podría acceder a las órdenes de trabajo. `Descartado` para el MVP.
- **OAuth / SSO (Google)** — Sería útil si el taller utilizara Google Workspace para su gestión administrativa, pero no hay evidencia de esta infraestructura. `Descartado`.
- **Acceso por URL secreta** — Inaceptable, ya que la agenda y el estado de las órdenes contienen información confidencial sobre los clientes y los vehículos. `Descartado`.
- **Sesión basada en cookie de servidor** — La elección de JWT se mantiene por facilitar la autenticación stateless, ideal para dispositivos móviles (celulares) utilizados en el taller.

---

## Consecuencias

**Lo que se gana:**
- Las rutas críticas del sistema quedan protegidas desde el lanzamiento del MVP.
- Autonomía total: el sistema no depende de proveedores externos de identidad (Google/Microsoft).
- El token JWT es fácilmente extensible para añadir roles si el taller crece en personal.

**El costo que se acepta:**
- La recuperación de contraseñas no está automatizada en el MVP. Si el personal olvida sus credenciales, el procedimiento será manual (reseteo técnico).
- El personal debe recordar sus credenciales; al ser solo dos personas (Carlos y el asesor), este riesgo se considera manejable y aceptable para el MVP.