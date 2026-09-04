# Índice del Backlog

**Origen:** `.context/architecture/prd.md`
**Fuentes del backlog:** documentación local y respaldo histórico de Confluence
**Decisiones vigentes para el próximo release:** `.context/PBI/decisiones-po-proximo-release.md`
**Jira canónico:** `https://jlb984.atlassian.net/` · Project Key `BJHB`
**GitHub canónico:** `https://github.com/jlb984/Banco-J-Home-Banking`
**Migración:** completada y documentada en `.context/PBI/migracion-caq-a-bjhb.md`
**Tipo de proyecto:** Brownfield
**Fecha:** 04/09/2026

| Epic | Stories | Refinadas | Inspeccionadas | Sin verificar | Estado de sincronización |
| :--- | :---: | :---: | :---: | :---: | :--- |
| BJHB-1 [Epic] Cuenta y activación del profesional | 4 | 4 | 4 | 4 | Sincronizado con BJHB |
| BJHB-3 [Epic] Agenda, disponibilidad y gestión de turnos | 5 | 5 | 5 | 5 | Sincronizado con BJHB |
| BJHB-4 [Epic] Página pública y auto-reserva | 4 | 4 | 4 | 4 | Sincronizado con BJHB |
| BJHB-5 [Epic] Cancelaciones y comunicaciones transaccionales | 5 | 5 | 5 | 4 | Sincronizado con BJHB |
| BJHB-6 [Epic] Clientes y límite freemium | 4 | 4 | 4 | 4 | Sincronizado con BJHB |

## Epics identificadas, pendientes de desglosar

* Ninguna: el backlog está desglosado entero.

## Pendiente de subir a Jira

* Ninguna. Las 22 Stories fueron resincronizadas con `BJHB` el 04/09/2026.

## Estado de refinamiento

**Última corrida:** 04/09/2026 · alcance: todas

| Resultado | Cantidad |
| :--- | ---: |
| Stories reprocesadas | 22 |
| Stories refinadas | 22 |
| Escenarios Gherkin vigentes | 157 |
| Escenarios netos agregados en esta corrida | 71 |
| Stories con preguntas funcionales abiertas | 0 |

Las decisiones vigentes de `.context/PBI/decisiones-po-proximo-release.md` se incorporaron
como reglas explícitas y escenarios verificables. Las preguntas que permanecen en cuatro
Stories son de diagnóstico técnico sobre discrepancias ya observadas, no decisiones
funcionales pendientes.

**Discrepancias contra Jira antes de la sincronización:** ninguna modificación funcional
más reciente que el material local. Se conservaron las claves reales de `BJHB`.

## Estado de Shift-Left Testing

**Última corrida:** 04/09/2026 · alcance: todas

| Resultado | Cantidad |
| :--- | ---: |
| Stories inspeccionadas | 22 |
| Aprobadas | 18 |
| Requiere cambios | 0 |
| Bloqueantes | 4 |
| Hallazgos abiertos | 4 |

Las decisiones de Producto resolvieron las 69 observaciones de requisitos de la corrida
anterior. Permanecen cuatro discrepancias contra comportamiento observado: protección después
del logout (`BJHB-11`), exposición de la URL pública (`BJHB-13`), cancelación no persistida
(`BJHB-24`) y alta manual de cliente sin respuesta visible (`BJHB-9`).

Los cinco planes de prueba por Epic fueron regenerados con la escala de riesgo vigente, los
objetivos no funcionales aprobados y la necesidad documentada de un entorno QA aislado. La
ejecución mutante continúa bloqueada mientras el mapa real solo confirme Producción y no existan
seed, teardown ni correo sandbox disponibles.

**Sincronización Jira:** completada y verificada el 04/09/2026. Las 22 descripciones fueron
actualizadas y cada Story conserva exactamente un comentario Shift-Left y uno de decisión de
Producto, ambos actualizados sin duplicados.

## Pendiente de verificar contra la aplicación

* Todas las Stories excepto BJHB-24 permanecen `Sin verificar` por tratarse de un proyecto
  Brownfield y existir únicamente un entorno de producción con datos reales.
* BJHB-24 fue verificada parcialmente en producción con un turno sintético: la UI inicia la cancelación, pero el cambio no persiste ni libera el horario.

## Contradicciones detectadas

* La especificación describe `cita.ai`, `uat.cita.ai` y dos bases separadas; `nota-ambientes-y-accesos.md`, más reciente, establece que solo está activa producción en `https://cita-ai.vercel.app/`. Se toma la nota del 21/05/2026 por ser posterior.
* La especificación y las notas técnicas describen el dashboard protegido por middleware; el PRD registra que, después de cerrar sesión, las rutas del dashboard continuaron mostrando sus pantallas. El comportamiento real de autorización permanece sin verificar.
* La especificación exige una contraseña de al menos ocho caracteres, una mayúscula y un número; no hay evidencia actual que confirme que la interfaz y Supabase apliquen exactamente esas tres validaciones.
* La especificación afirma que la URL pública se genera al registrarse, pero soporte y la observación del producto indican que no se muestra en el panel. Se conserva la generación como comportamiento documentado y su exposición como brecha.
* Las notas técnicas antiguas atribuyen todos los correos a Supabase; el hilo del 03/03/2026 establece que los correos de producto migraron a Resend y los de autenticación permanecieron en Supabase. Se toma el hilo más reciente.
* La especificación acepta cualquier duración entera positiva; la interfaz observada ofrece 15, 30, 45, 60, 90 y 120 minutos. No se elige una regla hasta que negocio la confirme.
* La especificación presenta el recordatorio del día anterior como requisito; el hilo del 03/03/2026 y la reunión del 19/05/2026 confirman que fue excluido del lanzamiento y sigue sin implementarse. Se conserva como brecha priorizada.
* La especificación exige impedir superposiciones, pero las notas técnicas documentan una validación no transaccional y soporte registró duplicados. El comportamiento esperado se conserva y la implementación queda sin verificar.
* El resumen automático atribuye ambos casos de turnos duplicados a husos horarios; la transcripción confirma esa causa solo para uno. Se toma la transcripción como fuente original.
* La especificación funcional exige que cancelar cambie el turno a `cancelled` y libere el horario; en BJHB-24 la UI retiró temporalmente el turno, pero una navegación nueva lo mostró otra vez, el endpoint autenticado lo mantuvo `confirmed` y la disponibilidad pública no liberó el slot.
* Se mezcló material de dos proyectos en una ejecución anterior. El 03/09/2026 se comparó
  contra `BJHB`, se evitaron duplicados y se migró el contenido a claves reales. El detalle
  queda en `.context/PBI/migracion-caq-a-bjhb.md`; el otro proyecto no debe volver a usarse
  como destino Jira de este repositorio.

## Preguntas abiertas

* ¿Debe bloquearse temporalmente una cuenta después de varios intentos fallidos de inicio de sesión? ¿Después de cuántos intentos?
* ¿El cierre de sesión debe redirigir inmediatamente al login y eliminar cualquier contenido del dashboard visible en el navegador?
* ¿Dónde y mediante qué interacción debe mostrarse la URL pública para cumplir la promesa de activación en menos de cinco minutos?
* ¿Qué sucede con los turnos existentes cuando el profesional bloquea el período que los contiene?
* ¿Cuál es la anticipación máxima para reservar y existe una ventana mínima de cancelación?
* ¿Qué zona horaria rige la agenda y cómo se presenta un turno a clientes ubicados en otros países?
* ¿La duración válida es cualquier entero positivo o únicamente una opción de la interfaz?
* ¿Debe existir el estado `No se presentó`?
* ¿Cómo se representa a quien reserva para otra persona?
* ¿Qué incluye el Plan Pro, cuánto cuesta y cómo se procesa una solicitud de información?
* ¿Cuáles son los objetivos acordados de rendimiento, disponibilidad, concurrencia, accesibilidad y compatibilidad?
* No se recorrió la aplicación en esta fase; solo existe un entorno de producción y el prompt de backlog no autoriza generar datos reales para verificar las Stories.
