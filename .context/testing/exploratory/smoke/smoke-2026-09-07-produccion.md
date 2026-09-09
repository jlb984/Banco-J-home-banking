# Reporte de Smoke Test

**Fecha:** 07/09/2026
**Entorno:** Producción
**URL:** `https://cita-ai.vercel.app/`
**Usuario:** Cuenta profesional de prueba según `.context/infrastructure/test-data-strategy.md` (contraseña en `.env` como `TEST_USER_PASSWORD`, ver `.env.example`; no se reproduce en este reporte)
**Modo de ejecución:** Playwright MCP
**Estado:** PASSED

## Verificaciones
| # | Verificación | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Carga de la portada | PASS | `evidence/smoke-2026-09-07-produccion-portada.png` |
| 2 | Título de la página | PASS | Título observado `Iniciar Sesión - CITA AI` |
| 3 | Inicio de sesión | No ejecutado | Sin envío de formulario (ver Sin cobertura) |

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| N/A — sanidad general | Carga de la portada y disponibilidad del formulario | — | `evidence/smoke-2026-09-07-produccion-portada.png` |
| BJHB-11 | Inicio de sesión válido y redirección al dashboard | — | No ejecutado en esta corrida |

## Sin cobertura
* No se envió el formulario de login: la contraseña vive en `.env` y no se abre ni se pide por chat; el autofill del navegador mostraba otra cuenta y no la de prueba documentada, por lo que enviarlo en Producción excedía el alcance del smoke.
* No se validaron escrituras, creación de cuentas, reservas, cancelaciones ni correos porque Producción es el único entorno confirmado y no dispone de teardown seguro.
* No se verificaron rutas internas más allá de la llegada a `/login`, ni compatibilidad, performance, accesibilidad o autorización profunda; quedan fuera del alcance de este smoke test.
