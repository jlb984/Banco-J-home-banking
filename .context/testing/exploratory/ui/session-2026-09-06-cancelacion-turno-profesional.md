# Sesión Exploratoria UI: Cancelación de un turno por el profesional

**Fecha:** 06/09/2026
**Entorno:** Producción
**URL:** `https://cita-ai.vercel.app/`
**Duración:** charter de 30 minutos; 13 minutos de interacción automatizada efectiva
**Modo de ejecución:** Playwright MCP
**Historia:** BJHB-24
**Misión:** encontrar inconsistencias en el flujo de cancelación desde el panel, aplicando las heurísticas Goldilocks, Super User y Back Button sin persistir datos ni disparar comunicaciones reales.

## Restricción del entorno

Producción es el único entorno confirmado y lo usan personas reales. La sesión se limitó a navegación, lectura y rechazo del diálogo de confirmación. No se confirmó ninguna cancelación ni se modificaron datos. Los valores visibles en las evidencias corresponden a turnos sintéticos ya existentes; no se registran datos personales reales.

## Escenarios probados

| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Acceder con la cuenta profesional guardada en el perfil y localizar turnos propios con acción `Cancelar` | PASS | `evidence/screenshots/2026-09-06-cancelacion-turno-profesional-turnos-con-accion-pass.png` |
| 2 | Iniciar la cancelación y verificar el texto exacto del diálogo | PASS | `evidence/cancelacion-turno-profesional-notes.md` |
| 3 | Rechazar el diálogo y comprobar que los tres turnos siguen visibles | PASS | `evidence/screenshots/2026-09-06-cancelacion-turno-profesional-abortar-confirmacion-pass.png` |
| 4 | Super User: abrir y rechazar dos veces seguidas la confirmación | PASS | `evidence/cancelacion-turno-profesional-notes.md` |
| 5 | Back Button: navegar a Disponibilidad, volver al dashboard y comprobar que las acciones siguen operativas | PASS | `evidence/cancelacion-turno-profesional-notes.md` |
| 6 | Visualizar las acciones en viewport móvil de 390 × 844 | PASS | `evidence/screenshots/2026-09-06-cancelacion-turno-profesional-vista-movil-pass.png` |
| 7 | Informar el motivo opcional definido para BJHB-24 | FAIL | `evidence/cancelacion-turno-profesional-notes.md` |
| 8 | Camino feliz completo: confirmar, persistir `cancelled`, retirar el turno y liberar el slot | No ejecutado | — |
| 9 | Rechazar un turno pasado o dentro de las 2 horas previas | No ejecutado | — |
| 10 | Rechazar la cancelación de un turno de otro profesional | No ejecutado | — |
| 11 | Goldilocks: motivo vacío, de 250/251 caracteres y con caracteres especiales | No ejecutado | — |

## Defectos encontrados

* **BJHB-28:** el único control presentado es un diálogo nativo de confirmación sin entrada de texto. El profesional no puede informar el motivo opcional de hasta 250 caracteres definido por Producto y por el escenario 6 de BJHB-24. La captura del diálogo no fue técnicamente posible porque Playwright bloquea screenshots mientras un diálogo nativo está abierto; el evento y su texto literal quedaron asentados en las notas.
* **Defecto conocido BJHB-24-D1:** la falta de persistencia y liberación observada el 02/09/2026 continúa abierta. No se reejecutó porque exige confirmar una escritura productiva y puede disparar un correo real.

## Trazabilidad

| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| BJHB-24 | Turnos futuros con acción de cancelación | — | `evidence/screenshots/2026-09-06-cancelacion-turno-profesional-turnos-con-accion-pass.png` |
| BJHB-24 | Texto y rechazo del diálogo de confirmación | — | `evidence/cancelacion-turno-profesional-notes.md`; `evidence/screenshots/2026-09-06-cancelacion-turno-profesional-abortar-confirmacion-pass.png` |
| BJHB-24 | Motivo opcional no disponible | BJHB-28 | `evidence/cancelacion-turno-profesional-notes.md` |
| BJHB-24 | Persistencia de `cancelled` y liberación del slot | BJHB-24-D1 (conocido) | Evidencia previa referenciada por la Story; no reejecutado en esta sesión |

## Sin cobertura

* La confirmación real, el estado persistido, la liberación pública del horario y el correo quedaron sin ejecutar por realizarse sobre Producción sin teardown ni sandbox de correo.
* No se dispuso de fixtures seguros para turno pasado, turno dentro de 2 horas ni turno ajeno.
* Goldilocks sobre el motivo no pudo ejecutarse porque la UI no presenta un campo para ingresarlo.
* No se probaron concurrencia, carga, compatibilidad entre navegadores ni acceso directo a base de datos.
* La evidencia visual se recortó a turnos sintéticos para excluir información identificable visible en otras áreas del dashboard.

## Transferencia UI -> API/DB

### Acciones disparadoras

* `Cancelar` -> abre un diálogo nativo con el texto esperado; rechazarlo no produjo una solicitud de cancelación ni un cambio visible.
* Aceptar la confirmación -> debería invocar `POST /api/appointments/[id]/cancel`; no ejecutado en esta sesión.
* Recargar o volver al dashboard -> debería consultar los turnos y excluir el turno ya persistido como `cancelled`; la comprobación posterior a una cancelación no se ejecutó.
* Consultar disponibilidad pública -> debería confirmar que el slot cancelado volvió a ofrecerse; no ejecutado.

### Datos de prueba utilizados

* profesional: cuenta de prueba ya guardada en el perfil del navegador; credencial no registrada.
* citas: tres turnos sintéticos futuros ya existentes, anonimizados como `Prueba QA 1`.
* appointmentId: no expuesto por la UI ni capturado, porque no se confirmó la operación.
* motivo: no ingresado; la UI no presentó un control para hacerlo.

### Trazas observadas

* requestId: no disponible.
* correlationId: no disponible.
* appointmentId: no disponible.
* La sesión no generó una solicitud de cancelación al rechazarse todos los diálogos.
