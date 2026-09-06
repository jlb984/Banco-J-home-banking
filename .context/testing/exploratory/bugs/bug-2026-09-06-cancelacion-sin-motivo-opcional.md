# Bug: [Cancelación UI] No permite informar el motivo opcional

**ID:** BJHB-28
**Estado de sincronización:** Sincronizado con Jira
**Fecha:** 06/09/2026
**Entorno:** Producción
**Severidad:** Media
**Prioridad:** Media

## Descripción

Al iniciar desde el dashboard la cancelación de un turno futuro propio, la aplicación presenta únicamente un diálogo nativo `confirm`. El profesional puede aceptar o rechazar la acción, pero no dispone de un control para informar el motivo opcional de hasta 250 caracteres definido para BJHB-24.

La cancelación principal puede iniciarse, por lo que el defecto no bloquea por sí solo todo el flujo. Sin embargo, impide cumplir una capacidad funcional aprobada y deja sin ejecutar las validaciones de longitud y caracteres del motivo.

## Pasos para reproducir

1. Iniciar sesión en `https://cita-ai.vercel.app/login` con la cuenta profesional de prueba almacenada en el perfil del navegador.
2. Ir a `/dashboard`.
3. Localizar un turno futuro propio en `Próximas Citas`.
4. Hacer clic en `Cancelar`.
5. Observar los controles disponibles en el diálogo de confirmación.

## Resultados

* **Esperado:** además de conservar la confirmación definida, la interfaz debe permitir informar opcionalmente un motivo de hasta 250 caracteres — según `.context/PBI/decisiones-po-proximo-release.md` · `BJHB-24 — Cancelación por el profesional` y `.context/PBI/epics/EPIC-BJHB-5-cancelaciones-y-comunicaciones-transaccionales/stories/STORY-BJHB-24-cancelacion-por-profesional/story.md` · `Escenario 6: Registrar un motivo opcional válido`.
* **Real:** se abre un diálogo nativo `confirm` con el texto `¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.` y solo permite aceptar o rechazar; no existe una entrada para el motivo.

## Evidencia

* `.context/testing/exploratory/ui/evidence/cancelacion-turno-profesional-notes.md`
* `.context/testing/exploratory/ui/evidence/screenshots/2026-09-06-cancelacion-turno-profesional-turnos-con-accion-pass.png`
* `.context/testing/exploratory/ui/evidence/screenshots/2026-09-06-cancelacion-turno-profesional-abortar-confirmacion-pass.png`

El diálogo es nativo del navegador y Playwright bloquea la captura de pantalla mientras está abierto. Su tipo y texto literal quedaron registrados por la automatización y asentados en la evidencia textual. Las capturas muestran los estados inmediatamente anterior y posterior al rechazo del diálogo.

## Entorno técnico

* Aplicación: Cita AI web responsive construida con Next.js.
* Entorno: Producción, único entorno operativo documentado.
* URL: `https://cita-ai.vercel.app/`.
* Navegador: Chromium administrado por Playwright MCP; versión no expuesta.
* Sistema operativo: Windows.
* Versión de la aplicación: no expuesta por la UI; deploy productivo vigente el 06/09/2026.

## Clasificación

* **Severidad Media:** la cancelación puede iniciarse, pero una regla funcional aprobada no tiene representación en la UI y no existe alternativa visible para cargar el dato.
* **Prioridad Media:** debe resolverse durante la estabilización del release 1.1 junto con BJHB-24, sin desplazar el defecto bloqueante conocido de persistencia y liberación del slot.

## Trazabilidad

| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| BJHB-24 | Escenario 6: registrar un motivo opcional válido | BJHB-28 | `.context/testing/exploratory/ui/evidence/cancelacion-turno-profesional-notes.md` |

## Referencia cruzada

* **Sesión de origen:** `.context/testing/exploratory/ui/session-2026-09-06-cancelacion-turno-profesional.md`
* **Datos de prueba usados:** cuenta profesional de prueba referenciada desde el perfil del navegador y turnos sintéticos preexistentes identificados como `Prueba QA 1`; sin credenciales ni datos personales reales.
* **IDs de traza:** `requestId`, `correlationId` y `appointmentId` no disponibles; la cancelación no se confirmó y no se generó la solicitud asociada.
* **Historia relacionada:** `BJHB-24`.
* **Issue Jira:** `https://jlb984.atlassian.net/browse/BJHB-28`.
* **Defecto relacionado:** `BJHB-24-D1`, bloqueo conocido de persistencia y liberación del slot; no reejecutado en esta sesión.
