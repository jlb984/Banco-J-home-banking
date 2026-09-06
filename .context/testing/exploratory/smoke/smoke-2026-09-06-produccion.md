# Reporte de Smoke Test

**Fecha:** 06/09/2026
**Entorno:** Producción
**URL:** `https://cita-ai.vercel.app/`
**Usuario:** Cuenta profesional de prueba almacenada en el perfil del navegador
**Modo de ejecución:** Playwright MCP
**Estado:** PASSED

## Verificaciones

| # | Verificación | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Carga de la portada | PASS | `evidence/2026-09-06-produccion-portada-login.png`; `evidence/2026-09-06-produccion-network-completo.log` |
| 2 | Título de la página | PASS | `evidence/2026-09-06-produccion-page-state.md` |
| 3 | Inicio de sesión | PASS | `evidence/2026-09-06-produccion-dashboard-login-exitoso.png`; `evidence/2026-09-06-produccion-page-state.md` |

## Observaciones

* La URL principal redirigió a `/login` y mostró el título `Iniciar Sesión - CITA AI`.
* El login con las credenciales guardadas en el perfil del navegador respondió `200` y redirigió a `/dashboard`, cuyo título fue `CITA AI - Gestión de Citas`.
* Antes del login, la aplicación intentó renovar una sesión anterior y Supabase respondió `400 Invalid Refresh Token`, generando dos errores de consola. El formulario permaneció operativo y la autenticación posterior fue exitosa; se registra como observación no bloqueante.
* No se observaron respuestas `500` ni `404` en las solicitudes dinámicas capturadas.
* El dashboard mostró la tarjeta `Tu enlace público de reservas`. Es evidencia nueva relacionada con `BJHB-13`, pero este smoke no verificó las acciones de abrir o copiar ni alcanza para cerrar la inspección bloqueante.

## Trazabilidad

| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| N/A — sanidad general | Carga de la portada y disponibilidad del formulario | — | `evidence/2026-09-06-produccion-portada-login.png` |
| BJHB-11 | Inicio de sesión válido y redirección al dashboard | — | `evidence/2026-09-06-produccion-dashboard-login-exitoso.png` |
| BJHB-13 | Presencia de la tarjeta del enlace público | — | `evidence/2026-09-06-produccion-dashboard-login-exitoso.png` |

## Sin cobertura

* No se validaron escrituras, creación de cuentas, reservas, cancelaciones ni correos porque Producción es el único entorno confirmado y no dispone de teardown seguro.
* No se verificaron las rutas internas más allá de la llegada al dashboard.
* No se ejecutaron `Abrir perfil` ni `Copiar enlace`; `BJHB-13` requiere una verificación específica antes de cambiar su estado.
* No se ejecutó compatibilidad entre navegadores, performance, accesibilidad ni autorización profunda; quedan fuera del alcance de este smoke test.
